---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Authentification et autorisation dans ASP.NET’API Web (en anglais seulement) Microsoft Docs
author: MikeWasson
description: Donne un aperçu général de l’authentification et de l’autorisation dans ASP.NET’API Web.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676257"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Authentication and Authorization in ASP.NET Web API (Authentification et autorisation dans l’API Web ASP.NET)

par [Mike Wasson](https://github.com/MikeWasson)

Vous avez créé une API Web, mais maintenant vous souhaitez contrôler l’accès à celui-ci. Dans cette série d’articles, nous examinerons certaines options pour sécuriser une API Web auprès d’utilisateurs non autorisés. Cette série couvrira à la fois l’authentification et l’autorisation.

- *L’authentification* est de connaître l’identité de l’utilisateur. Par exemple, Alice se connecte avec son nom d’utilisateur et son mot de passe, et le serveur utilise le mot de passe pour authentifier Alice.
- *L’autorisation* est de décider si un utilisateur est autorisé à effectuer une action. Par exemple, Alice a la permission d’obtenir une ressource, mais pas de créer une ressource.

Le premier article de la série donne un aperçu général de l’authentification et de l’autorisation dans ASP.NET’API Web. D’autres sujets décrivent des scénarios d’authentification courants pour l’API Web.

> [!NOTE]
> Merci aux personnes qui ont passé en revue cette série et fourni des commentaires précieux: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.

## <a name="authentication"></a>Authentification

L’API Web suppose que l’authentification se produit dans l’hôte. Pour l’hébergement web, l’hôte est IIS, qui utilise des modules HTTP pour l’authentification. Vous pouvez configurer votre projet pour utiliser l’un des modules d’authentification intégrés à l’IIS ou à ASP.NET, ou écrire votre propre module HTTP pour effectuer une authentification personnalisée.

Lorsque l’hôte authentifie l’utilisateur, il crée un *principal*, qui est un objet [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) qui représente le contexte de sécurité dans lequel le code est en cours d’exécution. L’hôte attache le principal au fil actuel en définissant **Thread.CurrentPrincipal**. Le principal contient un objet **d’identité** associé qui contient des informations sur l’utilisateur. Si l’utilisateur est authentifié, la propriété **Identity.IsAuthenticated** retourne **vrai**. Pour les demandes anonymes, **IsAuthenticated** renvoie **faux**. Pour plus d’informations sur les directeurs d’école, voir [Sécurité basée sur les rôles](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>HTTP Message Handlers pour l’authentification

Au lieu d’utiliser l’hôte pour l’authentification, vous pouvez mettre la logique d’authentification dans un [gestionnaire de message HTTP](../advanced/http-message-handlers.md). Dans ce cas, le gestionnaire de message examine la demande HTTP et fixe le directeur.

Quand devriez-vous utiliser des gestionnaires de messages pour l’authentification? Voici quelques compromis :

- Un module HTTP voit toutes les demandes qui passent par le pipeline ASP.NET. Un gestionnaire de message ne voit que les demandes qui sont acheminées vers l’API Web.
- Vous pouvez définir des gestionnaires de messages par itinéraire, ce qui vous permet d’appliquer un schéma d’authentification à un itinéraire spécifique.
- Les modules HTTP sont spécifiques à l’IIS. Les gestionnaires de messages sont agnostiques, de sorte qu’ils peuvent être utilisés à la fois avec l’hébergement Web et l’auto-hébergement.
- Les modules HTTP participent à l’enregistrement, à l’audit de l’IIS, etc.
- Les modules HTTP s’exécutent plus tôt dans le pipeline. Si vous gérez l’authentification dans un gestionnaire de message, le directeur ne se configure pas tant que le gestionnaire ne s’exécute pas. De plus, le directeur revient au principal précédent lorsque la réponse quitte le gestionnaire de message.

En général, si vous n’avez pas besoin de prendre en charge l’auto-hébergement, un module HTTP est une meilleure option. Si vous avez besoin de prendre en charge l’auto-hébergement, considérez un gestionnaire de message.

### <a name="setting-the-principal"></a>Définir le principal

Si votre application exécute une logique d’authentification personnalisée, vous devez définir le principe sur deux endroits :

- **Thread.CurrentPrincipal**. Cette propriété est le moyen standard de définir le principal du thread en .NET.
- **HttpContext.Current.User**. Cette propriété est spécifique à ASP.NET.

Le code suivant montre comment définir le principe :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Pour l’hébergement web, vous devez définir le directeur dans les deux endroits; sinon, le contexte de la sécurité peut devenir incohérent. Pour l’auto-hébergement, cependant, **HttpContext.Current** est nul. Pour vous assurer que votre code est hôte-agnostique, par conséquent, vérifiez l’annulation avant d’être affecté à **HttpContext.Current**, comme indiqué.

## <a name="authorization"></a>Autorisation

L’autorisation se fait plus tard dans le pipeline, plus près du contrôleur. Cela vous permet de faire des choix plus granulaires lorsque vous accordez l’accès aux ressources.

- *Les filtres d’autorisation* s’exécutent avant l’action du contrôleur. Si la demande n’est pas autorisée, le filtre renvoie une réponse d’erreur, et l’action n’est pas invoquée.
- Dans le cadre d’une action de contrôleur, vous pouvez obtenir le principal actuel de la propriété **ApiController.User.** Par exemple, vous pouvez filtrer une liste de ressources en fonction du nom d’utilisateur, en ne retournant que les ressources qui appartiennent à cet utilisateur.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Utilisation de l’attribut [Autoriser]

L’API Web fournit un filtre d’autorisation intégré, [AutoriserAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Ce filtre vérifie si l’utilisateur est authentifié. Si ce n’est pas le cas, il renvoie le code d’état HTTP 401 (non autorisé), sans invoquer l’action.

Vous pouvez appliquer le filtre à l’échelle mondiale, au niveau du contrôleur ou au niveau des actions individuelles.

**Globalement**: Pour restreindre l’accès pour chaque contrôleur d’API Web, ajoutez le filtre Autoriser à la liste des **filtres** globaux :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Contrôleur**: Pour restreindre l’accès d’un contrôleur spécifique, ajoutez le filtre comme attribut au contrôleur :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Action**: Pour restreindre l’accès à des actions spécifiques, ajoutez l’attribut à la méthode d’action :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternativement, vous pouvez restreindre le contrôleur et ensuite autoriser `[AllowAnonymous]` un accès anonyme à des actions spécifiques, en utilisant l’attribut. Dans l’exemple `Post` suivant, la méthode `Get` est limitée, mais la méthode permet un accès anonyme.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Dans les exemples précédents, le filtre permet à tout utilisateur authentifié d’accéder aux méthodes restreintes; seuls les utilisateurs anonymes sont tenus à l’écart. Vous pouvez également limiter l’accès à des utilisateurs spécifiques ou aux utilisateurs dans des rôles spécifiques :

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Le filtre **Autoriserattribute** pour les contrôleurs Web API est situé dans l’espace de nom **System.Web.Http.** Il existe un filtre similaire pour les contrôleurs MVC dans l’espace de nom **System.Web.Mvc,** qui n’est pas compatible avec les contrôleurs Web API.

### <a name="custom-authorization-filters"></a>Filtres d’autorisation personnalisés

Pour écrire un filtre d’autorisation personnalisé, dérivez de l’un de ces types :

- **AutoriserAttribute**. Étendre cette classe pour effectuer la logique d’autorisation basée sur l’utilisateur actuel et les rôles de l’utilisateur.
- **AutorisationFilterAttribute**. Étendre cette classe pour effectuer une logique d’autorisation synchrone qui n’est pas nécessairement basée sur l’utilisateur ou le rôle actuel.
- **IAuthorizationFilter**. Implémentez cette interface pour effectuer une logique d’autorisation asynchrone ; par exemple, si votre logique d’autorisation fait des appels asynchrones I/O ou réseau. (Si votre logique d’autorisation est liée au processeur, il est plus simple de dériver de **AuthorizationFilterAttribute**, parce que vous n’avez pas besoin d’écrire une méthode asynchrone.)

Le diagramme suivant montre la hiérarchie de classe pour la classe **Autoriser.**

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorisation à l’intérieur d’une action de contrôleur

Dans certains cas, vous pouvez autoriser une demande de procéder, mais changer le comportement en fonction du principal. Par exemple, les informations que vous retournez peuvent changer en fonction du rôle de l’utilisateur. Dans le cadre d’une méthode de contrôleur, vous pouvez obtenir le principal actuel de la propriété **ApiController.User.**

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
