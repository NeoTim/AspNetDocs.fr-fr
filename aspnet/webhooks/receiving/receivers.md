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
# <a name="aspnet-webhooks-receivers"></a>récepteurs WebHooks ASP.NET

Recevoir Des WebHooks dépend de qui est l’expéditeur. Parfois, il ya des étapes supplémentaires enregistrant un WebHook vérifier que l’abonné est vraiment à l’écoute. Certains WebHooks fournissent un modèle push-to-pull où la demande HTTP POST ne contient qu’une référence à l’information sur l’événement qui doit ensuite être récupérée indépendamment. Souvent, le modèle de sécurité varie beaucoup.

Le but de Microsoft ASP.NET WebHooks est de le rendre à la fois plus simple et plus cohérent pour filer votre API sans passer beaucoup de temps à trouver comment gérer une variante particulière de WebHooks.

Un récepteur WebHook est responsable de l’acceptation et de la vérification des webHooks d’un expéditeur particulier. Un récepteur WebHook peut prendre en charge n’importe quel nombre de WebHook, chacun avec sa propre configuration. Par exemple, le récepteur WebHook GitHub peut accepter des WebHooks à partir d’un certain nombre de dépôts GitHub.

## <a name="webhook-receiver-uris"></a>URIs récepteur WebHook

En installant Microsoft ASP.NET WebHooks, vous obtenez un contrôleur WebHook général qui accepte les demandes WebHook d’un nombre de services ouvert. Lorsqu’une demande arrive, elle choisit le récepteur approprié que vous avez installé pour traiter un expéditeur WebHook particulier.

L’URI de ce contrôleur est le WebHook URI que vous vous inscrivez avec le service et est du formulaire:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Pour des raisons de sécurité, de nombreux récepteurs WebHook exigent que l’URI est un *https* URI et dans certains cas, il doit également contenir un paramètre de requête supplémentaire qui est utilisé pour faire respecter que seule la partie visée peut envoyer Des WebHooks à l’URI ci-dessus.

Le `<receiver>` composant est le nom du `github` `slack`récepteur, par exemple ou .

*L’id* est un identifiant optionnel qui peut être utilisé pour identifier une configuration particulière de récepteur WebHook. Cela peut être utilisé pour enregistrer N WebHooks avec un récepteur particulier. Par exemple, les trois URL suivantes peuvent être utilisées pour s’inscrire à trois WebHook indépendants :

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Installation d’un récepteur WebHook

Pour recevoir des WebHooks à l’aide de Microsoft ASP.NET WebHooks, vous installez d’abord le forfait Nuget pour le fournisseur WebHook ou les fournisseurs dont vous souhaitez recevoir des WebHook. Les paquets Nuget sont [nommés Microsoft.AspNet.WebHooks.Receivers.](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) Par exemple

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fournit un support pour recevoir des WebHooks de GitHub et [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fournit un support pour recevoir des WebHooks générés par ASP.NET WebHooks.

Hors de la boîte, vous pouvez trouver un support pour Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, et WordPress, mais il est possible de prendre en charge un certain nombre d’autres fournisseurs.

## <a name="configuring-a-webhook-receiver"></a>Configurer un récepteur WebHook

Les récepteurs WebHook sont configurés via [l’inteface IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) et des implémentations particulières de cette interface peuvent être enregistrées à l’aide de n’importe quel modèle d’injection de dépendance. La implémentation par défaut utilise des paramètres d’application qui peuvent être définis dans le fichier Web.config, ou, si vous utilisez Azure Web Apps, peuvent être définis via le [portail Azure](https://portal.azure.com/).

![Paramètres de l’application dans Microsoft Azure](_static/AzureAppSettings.png)

Le format des clés de réglage d’application est le suivant :

```
MS_WebHookReceiverSecret_<receiver>
```

La valeur est une liste de valeurs séparées par virgule correspondant aux valeurs *« id »* pour lesquelles Les WebHook ont été enregistrés, par exemple :

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Initialiser un récepteur WebHook

Les récepteurs WebHook sont parasésiés en les inscrivant, généralement dans la classe statique *WebApiConfig,* par exemple :

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
