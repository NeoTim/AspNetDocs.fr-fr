---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing dans ASP.NET’API Web (fr) Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676131"
---
# <a name="routing-in-aspnet-web-api"></a>Routing in ASP.NET Web API (Routage dans l’API Web ASP.NET)

par [Mike Wasson](https://github.com/MikeWasson)

Cet article décrit comment ASP.NET’API Web achemine les demandes HTTP aux contrôleurs.

> [!NOTE]
> Si vous connaissez ASP.NET MVC, le routage de l’API Web est très similaire au routage MVC. La principale différence est que l’API Web utilise le verbe HTTP, et non le chemin URI, pour sélectionner l’action. Vous pouvez également utiliser le routage de style MVC dans l’API Web. Cet article n’assume aucune connaissance de ASP.NET MVC.

## <a name="routing-tables"></a>Tables de routage

Dans ASP.NET’API Web, un *contrôleur* est une classe qui traite les demandes HTTP. Les méthodes publiques du contrôleur sont appelées *méthodes d’action* ou simplement *des actions*. Lorsque le cadre de l’API Web reçoit une demande, il achemine la demande à une action.

Pour déterminer quelle action invoquer, le cadre utilise un *tableau de routage*. Le modèle de projet Visual Studio pour l’API Web crée un itinéraire par défaut :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Cet itinéraire est défini dans le fichier *WebApiConfig.cs,* qui est placé dans *l’annuaire\_App Start* :

![](routing-in-aspnet-web-api/_static/image1.png)

Pour plus d’informations sur la `WebApiConfig` classe, voir [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Si vous auto-hébergez l’API Web, vous `HttpSelfHostConfiguration` devez régler la table de routage directement sur l’objet. Pour plus d’informations, voir [Self-Host a Web API](../older-versions/self-host-a-web-api.md).

Chaque entrée dans le tableau de routage contient un *modèle d’itinéraire*. Le modèle d’itinéraire par &quot;défaut pour l’API&quot;Web est api/'controller'/id' . Dans ce &quot;modèle,&quot; api est un segment de chemin littéral, et «contrôleur» et «id» sont des variables placeholder.

Lorsque le cadre Web API reçoit une demande HTTP, il essaie de faire correspondre l’URI à l’un des modèles d’itinéraire dans la table de routage. Si aucun itinéraire ne correspond, le client reçoit une erreur de 404. Par exemple, les URL suivantes correspondent à l’itinéraire par défaut :

- /api/contacts
- /api/contacts/1
- /api/produits/gizmo1

Cependant, l’URI suivante ne correspond pas, car il manque le &quot;segment api:&quot;

- /contacts/1

> [!NOTE]
> La raison de l’utilisation de "api" dans l’itinéraire est d’éviter les collisions avec ASP.NET itinéraire MVC. De cette façon, &quot;vous&quot; pouvez avoir /contacts &quot;aller à un&quot; contrôleur MVC, et /api / contacts aller à un contrôleur d’API Web. Bien sûr, si vous n’aimez pas cette convention, vous pouvez modifier le tableau d’itinéraire par défaut.

Une fois qu’un itinéraire correspondant est trouvé, l’API Web sélectionne le contrôleur et l’action :

- Pour trouver le contrôleur, &quot;l’API Web ajoute controller&quot; à la valeur de la variable *« contrôleur* ».
- Pour trouver l’action, Web API regarde le verbe HTTP, puis cherche une action dont le nom commence par ce nom de verbe HTTP. Par exemple, avec une demande GET, Web API &quot;recherche&quot;une &quot;action préfixée &quot;avec Get ,&quot;comme GetContact&quot; ou GetAllContacts . Cette convention ne s’applique qu’aux verbes GET, POST, PUT, DELETE, HEAD, OPTIONS et PATCH. Vous pouvez activer d’autres verbes HTTP en utilisant des attributs sur votre contrôleur. Nous verrons un exemple de cela plus tard.
- D’autres variables de place dans le modèle d’itinéraire, telles que « id », sont cartographiées en paramètres *d’action.*

Intéressons-nous à un exemple. Supposons que vous définissiez le contrôleur suivant :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Voici quelques demandes http://s d’accueil possibles, ainsi que l’action qui est invoquée pour chacune :

| Verbe HTTP | Chemin URI | Action | Paramètre |
| --- | --- | --- | --- |
| GET | api/produits | GetAllProducts (en) | *(aucun)* |
| GET | api/produits/4 | GetProductById GetProductById | 4 |
| Suppression | api/produits/4 | DeleteProduct (DeleteProduct) | 4 |
| POST | api/produits | *(pas de correspondance)* |  |

Notez que le segment *«id»* de l’URI, s’il est présent, est cartographié sur le paramètre *d’identification* de l’action. Dans cet exemple, le contrôleur définit deux méthodes GET, l’une avec un *paramètre d’identification* et l’autre sans paramètres.

Aussi, notez que la demande POST échouera, &quot;parce que le contrôleur ne définit pas un poste ... &quot; méthode.

## <a name="routing-variations"></a>Variations de routage

La section précédente décrivait le mécanisme de routage de base pour ASP.NET’API Web. Cette section décrit certaines variations.

### <a name="http-verbs"></a>Verbes HTTP

Au lieu d’utiliser la convention de nommage pour les verbes HTTP, vous pouvez spécifier explicitement le verbe HTTP pour une action en décorant la méthode d’action avec l’un des attributs suivants:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Dans l’exemple `FindProduct` suivant, la méthode est cartographiée pour les demandes GET :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Pour permettre plusieurs verbes HTTP pour une action, ou pour permettre des verbes HTTP autres que `[AcceptVerbs]` GET, PUT, POST, DELETE, HEAD, OPTIONS, et PATCH, utilisez l’attribut, qui prend une liste de verbes HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routage par nom d’action

Avec le modèle de routage par défaut, l’API Web utilise le verbe HTTP pour sélectionner l’action. Cependant, vous pouvez également créer un itinéraire où le nom d’action est inclus dans l’URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

Dans ce modèle d’itinéraire, le paramètre *« action »* nomme la méthode d’action sur le contrôleur. Avec ce style de routage, utilisez des attributs pour spécifier les verbes HTTP autorisés. Supposons, par exemple, que votre contrôleur ait la méthode suivante :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

Dans ce cas, une demande GET pour "api/produits/détails/1" serait la carte de la `Details` méthode. Ce style de routage est similaire à ASP.NET MVC, et peut être approprié pour une API de type RPC.

Vous pouvez remplacer le nom `[ActionName]` d’action en utilisant l’attribut. Dans l’exemple suivant, il ya &quot;deux actions qui cartographient à api / produits / vignette /*id*. L’un prend en charge GET et l’autre prend en charge POST :

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Non-Actions

Pour éviter qu’une méthode ne soit `[NonAction]` invoquée comme action, utilisez l’attribut. Cela indique au cadre que la méthode n’est pas une action, même si elle correspondrait autrement aux règles d’acheminement.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Pour aller plus loin

Ce sujet a fourni une vue de haut niveau de l’itinéraire. Pour plus de détails, voir [Routing et Action Selection](routing-and-action-selection.md), qui décrit exactement comment le cadre correspond à un URI à un itinéraire, sélectionne un contrôleur, puis sélectionne l’action à invoquer.
