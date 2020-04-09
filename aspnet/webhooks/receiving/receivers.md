---
uid: webhooks/receiving/receivers
title: ASP.NET récepteurs WebHooks (fr) Microsoft Docs
author: rick-anderson
description: récepteurs WebHooks ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 60f46141b59fc3888a6480d8201160420469d1a7
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675167"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="18a56-103">récepteurs WebHooks ASP.NET</span><span class="sxs-lookup"><span data-stu-id="18a56-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="18a56-104">Recevoir Des WebHooks dépend de qui est l’expéditeur.</span><span class="sxs-lookup"><span data-stu-id="18a56-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="18a56-105">Parfois, il ya des étapes supplémentaires enregistrant un WebHook vérifier que l’abonné est vraiment à l’écoute.</span><span class="sxs-lookup"><span data-stu-id="18a56-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="18a56-106">Certains WebHooks fournissent un modèle push-to-pull où la demande HTTP POST ne contient qu’une référence à l’information sur l’événement qui doit ensuite être récupérée indépendamment.</span><span class="sxs-lookup"><span data-stu-id="18a56-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="18a56-107">Souvent, le modèle de sécurité varie beaucoup.</span><span class="sxs-lookup"><span data-stu-id="18a56-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="18a56-108">Le but de Microsoft ASP.NET WebHooks est de le rendre à la fois plus simple et plus cohérent pour filer votre API sans passer beaucoup de temps à trouver comment gérer une variante particulière de WebHooks.</span><span class="sxs-lookup"><span data-stu-id="18a56-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="18a56-109">Un récepteur WebHook est responsable de l’acceptation et de la vérification des webHooks d’un expéditeur particulier.</span><span class="sxs-lookup"><span data-stu-id="18a56-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="18a56-110">Un récepteur WebHook peut prendre en charge n’importe quel nombre de WebHook, chacun avec sa propre configuration.</span><span class="sxs-lookup"><span data-stu-id="18a56-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="18a56-111">Par exemple, le récepteur WebHook GitHub peut accepter des WebHooks à partir d’un certain nombre de dépôts GitHub.</span><span class="sxs-lookup"><span data-stu-id="18a56-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="18a56-112">URIs récepteur WebHook</span><span class="sxs-lookup"><span data-stu-id="18a56-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="18a56-113">En installant Microsoft ASP.NET WebHooks, vous obtenez un contrôleur WebHook général qui accepte les demandes WebHook d’un nombre de services ouvert.</span><span class="sxs-lookup"><span data-stu-id="18a56-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="18a56-114">Lorsqu’une demande arrive, elle choisit le récepteur approprié que vous avez installé pour traiter un expéditeur WebHook particulier.</span><span class="sxs-lookup"><span data-stu-id="18a56-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="18a56-115">L’URI de ce contrôleur est le WebHook URI que vous vous inscrivez avec le service et est du formulaire:</span><span class="sxs-lookup"><span data-stu-id="18a56-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="18a56-116">Pour des raisons de sécurité, de nombreux récepteurs WebHook exigent que l’URI est un *https* URI et dans certains cas, il doit également contenir un paramètre de requête supplémentaire qui est utilisé pour faire respecter que seule la partie visée peut envoyer Des WebHooks à l’URI ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="18a56-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="18a56-117">Le `<receiver>` composant est le nom du `github` `slack`récepteur, par exemple ou .</span><span class="sxs-lookup"><span data-stu-id="18a56-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="18a56-118">*L’id* est un identifiant optionnel qui peut être utilisé pour identifier une configuration particulière de récepteur WebHook.</span><span class="sxs-lookup"><span data-stu-id="18a56-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="18a56-119">Cela peut être utilisé pour enregistrer N WebHooks avec un récepteur particulier.</span><span class="sxs-lookup"><span data-stu-id="18a56-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="18a56-120">Par exemple, les trois URL suivantes peuvent être utilisées pour s’inscrire à trois WebHook indépendants :</span><span class="sxs-lookup"><span data-stu-id="18a56-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="18a56-121">Installation d’un récepteur WebHook</span><span class="sxs-lookup"><span data-stu-id="18a56-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="18a56-122">Pour recevoir des WebHooks à l’aide de Microsoft ASP.NET WebHooks, vous installez d’abord le forfait Nuget pour le fournisseur WebHook ou les fournisseurs dont vous souhaitez recevoir des WebHook.</span><span class="sxs-lookup"><span data-stu-id="18a56-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="18a56-123">Les paquets Nuget sont [nommés Microsoft.AspNet.WebHooks.Receivers.](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)</span><span class="sxs-lookup"><span data-stu-id="18a56-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="18a56-124">Par exemple</span><span class="sxs-lookup"><span data-stu-id="18a56-124">For example</span></span>

<span data-ttu-id="18a56-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fournit un support pour recevoir des WebHooks de GitHub et [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fournit un support pour recevoir des WebHooks générés par ASP.NET WebHooks.</span><span class="sxs-lookup"><span data-stu-id="18a56-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="18a56-126">Hors de la boîte, vous pouvez trouver un support pour Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, et WordPress, mais il est possible de prendre en charge un certain nombre d’autres fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="18a56-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="18a56-127">Configurer un récepteur WebHook</span><span class="sxs-lookup"><span data-stu-id="18a56-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="18a56-128">Les récepteurs WebHook sont configurés via [l’inteface IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) et des implémentations particulières de cette interface peuvent être enregistrées à l’aide de n’importe quel modèle d’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="18a56-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="18a56-129">La implémentation par défaut utilise des paramètres d’application qui peuvent être définis dans le fichier Web.config, ou, si vous utilisez Azure Web Apps, peuvent être définis via le [portail Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="18a56-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Paramètres de l’application dans Microsoft Azure](_static/AzureAppSettings.png)

<span data-ttu-id="18a56-131">Le format des clés de réglage d’application est le suivant :</span><span class="sxs-lookup"><span data-stu-id="18a56-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="18a56-132">La valeur est une liste de valeurs séparées par virgule correspondant aux valeurs *« id »* pour lesquelles Les WebHook ont été enregistrés, par exemple :</span><span class="sxs-lookup"><span data-stu-id="18a56-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="18a56-133">Initialiser un récepteur WebHook</span><span class="sxs-lookup"><span data-stu-id="18a56-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="18a56-134">Les récepteurs WebHook sont parasésiés en les inscrivant, généralement dans la classe statique *WebApiConfig,* par exemple :</span><span class="sxs-lookup"><span data-stu-id="18a56-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
