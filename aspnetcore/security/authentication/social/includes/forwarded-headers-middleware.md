---
ms.openlocfilehash: d7ef9b11af8ee11e4ec84404f8cdeb0b89384a3c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57130448"
---
## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a><span data-ttu-id="e5a28-101">Avant de demander des informations avec un proxy ou de l’équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="e5a28-101">Forward request information with a proxy or load balancer</span></span>

<span data-ttu-id="e5a28-102">Si l’application est déployée derrière un serveur proxy ou un équilibreur de charge, certaines informations de demande d’origine peuvent être transféré vers l’application dans les en-têtes de demande.</span><span class="sxs-lookup"><span data-stu-id="e5a28-102">If the app is deployed behind a proxy server or load balancer, some of the original request information might be forwarded to the app in request headers.</span></span> <span data-ttu-id="e5a28-103">Ces informations comprennent généralement le schéma de demande sécurisé (`https`), hôte et adresse IP du client.</span><span class="sxs-lookup"><span data-stu-id="e5a28-103">This information usually includes the secure request scheme (`https`), host, and client IP address.</span></span> <span data-ttu-id="e5a28-104">Les applications ne lisent pas automatiquement ces en-têtes de demande pour découvrir et d’utiliser les informations de demande d’origine.</span><span class="sxs-lookup"><span data-stu-id="e5a28-104">Apps don't automatically read these request headers to discover and use the original request information.</span></span>

<span data-ttu-id="e5a28-105">Le schéma est utilisé dans la génération de lien qui affecte le flux d’authentification auprès de fournisseurs externes.</span><span class="sxs-lookup"><span data-stu-id="e5a28-105">The scheme is used in link generation that affects the authentication flow with external providers.</span></span> <span data-ttu-id="e5a28-106">Perdre le schéma sécurisé (`https`) entraîne l’application de génération des URL de redirection non sécurisé incorrect.</span><span class="sxs-lookup"><span data-stu-id="e5a28-106">Losing the secure scheme (`https`) results in the app generating incorrect insecure redirect URLs.</span></span>

<span data-ttu-id="e5a28-107">Utilisez intergiciel des en-têtes transférés pour rendre les informations de demande d’origine disponible pour l’application pour traiter une demande.</span><span class="sxs-lookup"><span data-stu-id="e5a28-107">Use Forwarded Headers Middleware to make the original request information available to the app for request processing.</span></span>

<span data-ttu-id="e5a28-108">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="e5a28-108">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>
