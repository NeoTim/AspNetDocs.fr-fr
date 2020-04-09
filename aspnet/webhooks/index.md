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
# <a name="aspnet-webhooks-overview"></a>vue d’ensemble de ASP.NET WebHooks

WebHooks est un modèle HTTP léger fournissant un simple pub/ sous-modèle pour le câblage ensemble des API Web et des services SaaS. Lorsqu’un événement se produit dans un service, une notification est envoyée sous la forme d’une demande HTTP POST aux abonnés inscrits. La demande POST contient des informations sur l’événement qui permet au récepteur d’agir en conséquence.

En raison de leur simplicité, WebHooks sont déjà exposés par un grand nombre de services, y compris [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), et beaucoup plus. Par exemple, un WebHook peut indiquer qu’un fichier a changé dans [Dropbox](http://dropbox.com/), ou un changement de code a été commis dans GitHub, ou un paiement a été lancé dans [PayPal](http://www.paypal.com/), ou une carte a été créée dans [Trello](http://www.trello.com/). Les possibilités sont infinies!

Microsoft ASP.NET WebHooks facilite l’envoi et la réception de WebHooks dans le cadre de votre application ASP.NET :

* Du côté de la réception, il fournit un modèle commun pour recevoir et traiter les WebHook de n’importe quel nombre de fournisseurs de WebHook. Il sort de la boîte avec le soutien de [Dropbox](http://dropbox.com/), [GitHub](https://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) et [Zendesk,](https://www.zendesk.com/) mais il est facile d’ajouter un soutien pour plus.

* Du côté de l’envoi, il fournit un soutien pour la gestion et le stockage des abonnements ainsi que pour l’envoi de notifications d’événements à la bonne série d’abonnés. Cela vous permet de définir votre propre ensemble d’événements auxquels les abonnés peuvent s’abonner et les avertir lorsque les choses se produisent.

Les deux parties peuvent être utilisées ensemble ou séparées selon votre scénario. Si vous n’avez besoin que de recevoir des WebHooks d’autres services, vous pouvez utiliser uniquement la partie récepteur; si vous voulez seulement exposer WebHooks pour les autres à consommer, alors vous pouvez le faire.

Le code cible ASP.NET Web API 2 et ASP.NET MVC 5 et est disponible en tant [qu’OSS sur GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Vue d’ensemble WebHooks

WebHooks est un modèle qui signifie qu’il varie la façon dont il est utilisé d’un service à l’autre, mais l’idée de base est la même. Vous pouvez considérer WebHooks comme un simple pub/sous-modèle où un utilisateur peut s’abonner à des événements qui se déroulent ailleurs. Les notifications d’événements sont propagées sous forme de demandes HTTP POST contenant des informations sur l’événement lui-même.

En règle générale, la demande HTTP POST contient un objet JSON ou des données de formulaire HTML déterminées par l’expéditeur WebHook, y compris des informations sur l’événement provoquant le déclenchement du WebHook. Par exemple, un organisme de demande WebHook POST de [GitHub](https://www.github.com/) ressemble à ceci à la suite d’un nouveau numéro ouvert dans un dépôt particulier :

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

Pour s’assurer que le WebHook provient bel et bien de l’expéditeur prévu, la demande POST est sécurisée d’une manière ou d’une autre, puis vérifiée par le récepteur. Par exemple, [GitHub WebHooks](https://developer.github.com/webhooks/) comprend un en-tête *X-Hub-Signature* HTTP avec un hachage du corps de demande qui est vérifié par l’implémentation du récepteur de sorte que vous n’avez pas à vous inquiéter à ce sujet.

Le flux WebHook va généralement quelque chose comme ceci:

* L’expéditeur WebHook expose les événements auxquels un client peut s’abonner. Les événements décrivent des changements observables au système, par exemple qu’un nouvel élément de données a été inséré, qu’un processus a terminé, ou autre chose.

* Le récepteur WebHook s’abonner en enregistrant un WebHook composé de quatre choses :

     1. Une URI pour l’endroit où la notification de l’événement doit être affichée sous la forme d’une demande HTTP POST;

     2. Un ensemble de filtres décrivant les événements particuliers pour lesquels le WebHook devrait être tiré;

     3. Une clé secrète qui est utilisée pour signer la demande HTTP POST;

     4. Données supplémentaires qui doivent être incluses dans la demande HTTP POST. Cela peut par exemple être des champs d’en-tête HTTP supplémentaires ou des propriétés incluses dans l’organisme de demande HTTP POST.

* Une fois qu’un événement se produit, les inscriptions WebHook correspondantes sont trouvées et les demandes HTTP POST sont soumises. En règle générale, la génération des demandes HTTP POST sont rejugées à plusieurs reprises si, pour une raison quelconque, le destinataire ne répond pas ou si la demande HTTP POST entraîne une réponse d’erreur.

## <a name="webhooks-processing-pipeline"></a>Pipeline de traitement WebHooks

Le pipeline de traitement WebHooks de Microsoft ASP.NET pour les WebHook entrants ressemble à ceci :

![pipeline de traitement webHooks ASP.NET](_static/WebHookReceivers.png)

Les deux concepts clés ici sont *les récepteurs* et *les manutentionnaires*:

* *Les récepteurs* sont responsables de la manipulation de la saveur particulière de WebHook à partir d’un expéditeur donné et de l’application des contrôles de sécurité pour s’assurer que la demande WebHook est en effet de l’expéditeur prévu.

* *Les gestionnaires* sont généralement l’endroit où le code utilisateur exécute le traitement de la WebHook particulière.

Dans les nœuds suivants, ces concepts sont décrits plus en détail.
