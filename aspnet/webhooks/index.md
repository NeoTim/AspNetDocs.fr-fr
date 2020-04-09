---
uid: webhooks/index
title: ASP.NET Aperçu de WebHooks (fr) Microsoft Docs
author: rick-anderson
description: Une introduction à ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: aa5fa190386ec803a6801de2d815c948677fe1f5
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675443"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="86243-103">vue d’ensemble de ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="86243-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="86243-104">WebHooks est un modèle HTTP léger fournissant un simple pub/ sous-modèle pour le câblage ensemble des API Web et des services SaaS.</span><span class="sxs-lookup"><span data-stu-id="86243-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="86243-105">Lorsqu’un événement se produit dans un service, une notification est envoyée sous la forme d’une demande HTTP POST aux abonnés inscrits.</span><span class="sxs-lookup"><span data-stu-id="86243-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="86243-106">La demande POST contient des informations sur l’événement qui permet au récepteur d’agir en conséquence.</span><span class="sxs-lookup"><span data-stu-id="86243-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="86243-107">En raison de leur simplicité, WebHooks sont déjà exposés par un grand nombre de services, y compris [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), et beaucoup plus.</span><span class="sxs-lookup"><span data-stu-id="86243-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="86243-108">Par exemple, un WebHook peut indiquer qu’un fichier a changé dans [Dropbox](http://dropbox.com/), ou un changement de code a été commis dans GitHub, ou un paiement a été lancé dans [PayPal](http://www.paypal.com/), ou une carte a été créée dans [Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="86243-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="86243-109">Les possibilités sont infinies!</span><span class="sxs-lookup"><span data-stu-id="86243-109">The possibilities are endless!</span></span>

<span data-ttu-id="86243-110">Microsoft ASP.NET WebHooks facilite l’envoi et la réception de WebHooks dans le cadre de votre application ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="86243-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="86243-111">Du côté de la réception, il fournit un modèle commun pour recevoir et traiter les WebHook de n’importe quel nombre de fournisseurs de WebHook.</span><span class="sxs-lookup"><span data-stu-id="86243-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="86243-112">Il sort de la boîte avec le soutien de [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) et [Zendesk,](https://www.zendesk.com/) mais il est facile d’ajouter un soutien pour plus.</span><span class="sxs-lookup"><span data-stu-id="86243-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="86243-113">Du côté de l’envoi, il fournit un soutien pour la gestion et le stockage des abonnements ainsi que pour l’envoi de notifications d’événements à la bonne série d’abonnés.</span><span class="sxs-lookup"><span data-stu-id="86243-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="86243-114">Cela vous permet de définir votre propre ensemble d’événements auxquels les abonnés peuvent s’abonner et les avertir lorsque les choses se produisent.</span><span class="sxs-lookup"><span data-stu-id="86243-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="86243-115">Les deux parties peuvent être utilisées ensemble ou séparées selon votre scénario.</span><span class="sxs-lookup"><span data-stu-id="86243-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="86243-116">Si vous n’avez besoin que de recevoir des WebHooks d’autres services, vous pouvez utiliser uniquement la partie récepteur; si vous voulez seulement exposer WebHooks pour les autres à consommer, alors vous pouvez le faire.</span><span class="sxs-lookup"><span data-stu-id="86243-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="86243-117">Le code cible ASP.NET Web API 2 et ASP.NET MVC 5 et est disponible en tant [qu’OSS sur GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="86243-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="86243-118">Vue d’ensemble WebHooks</span><span class="sxs-lookup"><span data-stu-id="86243-118">WebHooks Overview</span></span>

<span data-ttu-id="86243-119">WebHooks est un modèle qui signifie qu’il varie la façon dont il est utilisé d’un service à l’autre, mais l’idée de base est la même.</span><span class="sxs-lookup"><span data-stu-id="86243-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="86243-120">Vous pouvez considérer WebHooks comme un simple pub/sous-modèle où un utilisateur peut s’abonner à des événements qui se déroulent ailleurs.</span><span class="sxs-lookup"><span data-stu-id="86243-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="86243-121">Les notifications d’événements sont propagées sous forme de demandes HTTP POST contenant des informations sur l’événement lui-même.</span><span class="sxs-lookup"><span data-stu-id="86243-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="86243-122">En règle générale, la demande HTTP POST contient un objet JSON ou des données de formulaire HTML déterminées par l’expéditeur WebHook, y compris des informations sur l’événement provoquant le déclenchement du WebHook.</span><span class="sxs-lookup"><span data-stu-id="86243-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="86243-123">Par exemple, un organisme de demande WebHook POST de [GitHub](https://www.github.com/) ressemble à ceci à la suite d’un nouveau numéro ouvert dans un dépôt particulier :</span><span class="sxs-lookup"><span data-stu-id="86243-123">For example, a WebHook POST request body from [GitHub](https://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="86243-124">Pour s’assurer que le WebHook provient bel et bien de l’expéditeur prévu, la demande POST est sécurisée d’une manière ou d’une autre, puis vérifiée par le récepteur.</span><span class="sxs-lookup"><span data-stu-id="86243-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="86243-125">Par exemple, [GitHub WebHooks](https://developer.github.com/webhooks/) comprend un en-tête *X-Hub-Signature* HTTP avec un hachage du corps de demande qui est vérifié par l’implémentation du récepteur de sorte que vous n’avez pas à vous inquiéter à ce sujet.</span><span class="sxs-lookup"><span data-stu-id="86243-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="86243-126">Le flux WebHook va généralement quelque chose comme ceci:</span><span class="sxs-lookup"><span data-stu-id="86243-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="86243-127">L’expéditeur WebHook expose les événements auxquels un client peut s’abonner.</span><span class="sxs-lookup"><span data-stu-id="86243-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="86243-128">Les événements décrivent des changements observables au système, par exemple qu’un nouvel élément de données a été inséré, qu’un processus a terminé, ou autre chose.</span><span class="sxs-lookup"><span data-stu-id="86243-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="86243-129">Le récepteur WebHook s’abonner en enregistrant un WebHook composé de quatre choses :</span><span class="sxs-lookup"><span data-stu-id="86243-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="86243-130">Une URI pour l’endroit où la notification de l’événement doit être affichée sous la forme d’une demande HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="86243-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="86243-131">Un ensemble de filtres décrivant les événements particuliers pour lesquels le WebHook devrait être tiré;</span><span class="sxs-lookup"><span data-stu-id="86243-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="86243-132">Une clé secrète qui est utilisée pour signer la demande HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="86243-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="86243-133">Données supplémentaires qui doivent être incluses dans la demande HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="86243-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="86243-134">Cela peut par exemple être des champs d’en-tête HTTP supplémentaires ou des propriétés incluses dans l’organisme de demande HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="86243-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="86243-135">Une fois qu’un événement se produit, les inscriptions WebHook correspondantes sont trouvées et les demandes HTTP POST sont soumises.</span><span class="sxs-lookup"><span data-stu-id="86243-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="86243-136">En règle générale, la génération des demandes HTTP POST sont rejugées à plusieurs reprises si, pour une raison quelconque, le destinataire ne répond pas ou si la demande HTTP POST entraîne une réponse d’erreur.</span><span class="sxs-lookup"><span data-stu-id="86243-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="86243-137">Pipeline de traitement WebHooks</span><span class="sxs-lookup"><span data-stu-id="86243-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="86243-138">Le pipeline de traitement WebHooks de Microsoft ASP.NET pour les WebHook entrants ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="86243-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![pipeline de traitement webHooks ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="86243-140">Les deux concepts clés ici sont *les récepteurs* et *les manutentionnaires*:</span><span class="sxs-lookup"><span data-stu-id="86243-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="86243-141">*Les récepteurs* sont responsables de la manipulation de la saveur particulière de WebHook à partir d’un expéditeur donné et de l’application des contrôles de sécurité pour s’assurer que la demande WebHook est en effet de l’expéditeur prévu.</span><span class="sxs-lookup"><span data-stu-id="86243-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="86243-142">*Les gestionnaires* sont généralement l’endroit où le code utilisateur exécute le traitement de la WebHook particulière.</span><span class="sxs-lookup"><span data-stu-id="86243-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="86243-143">Dans les nœuds suivants, ces concepts sont décrits plus en détail.</span><span class="sxs-lookup"><span data-stu-id="86243-143">In the following nodes these concepts are described in more details.</span></span>
