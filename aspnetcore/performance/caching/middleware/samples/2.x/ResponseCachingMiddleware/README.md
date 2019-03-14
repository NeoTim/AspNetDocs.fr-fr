---
ms.openlocfilehash: 93bda587eebc438e5da36b07cb7e4a37df8a91eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062716"
---
# <a name="aspnet-core-response-caching-sample"></a>Exemple de la mise en cache de réponse ASP.NET Core

Cet exemple illustre l’utilisation d’ASP.NET Core [intergiciel de mise en cache des réponses](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

L’application répond avec sa page d’Index, y compris un `Cache-Control` en-tête pour configurer le comportement de mise en cache. L’application définit également la `Vary` en-tête pour configurer le cache pour répondre à la réponse uniquement si le `Accept-Encoding` en-tête des demandes suivantes correspondent à ceux de la demande d’origine.

Lorsque vous exécutez l’exemple, la page d’Index est pris en charge à partir du cache lorsque stockés et mis en cache pendant 10 secondes.
