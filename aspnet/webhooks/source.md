---
uid: webhooks/source
title: ASP.NET code source WebHooks et forfaits NuGet Microsoft Docs
author: rick-anderson
description: Liens vers ASP.NET code source WebHooks et forfaits NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ad368125878871c0e38f35152c86fe4eea143924
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675323"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET code source WebHooks et forfaits NuGet

Microsoft ASP.NET WebHooks fait partie de la famille de modules Microsoft ASP.NET et est hébergé comme un [projet Open Source sur GitHub](https://github.com/aspnet/WebHooks). Cela signifie que nous acceptons les contributions, mais s’il vous plaît regarder les [lignes directrices sur](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) la contribution avant de soumettre une demande de retrait.

Cette documentation en ligne que vous lisez maintenant est également hébergée sous le titre [Open Source sur GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) et accepte également les contributions.

## <a name="nuget-packages"></a>Packages NuGet

Les [paquets NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sont divisés en trois parties :

* [Commun](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Un paquet commun qui est partagé entre les expéditeurs et les récepteurs.

* [Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Un ensemble de paquets qui soutiennent l’envoi de vos propres WebHooks à d’autres. La fonctionnalité pour l’envoi de WebHooks est décrite plus en détail dans [l’envoi de WebHooks](sending/senders.md).

* [Récepteurs](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Un ensemble de paquets qui favorisent la réception de WebHooks d’autres personnes. La fonctionnalité pour recevoir WebHooks est décrite plus en détail dans [Recevoir WebHooks](receiving/index.md).
