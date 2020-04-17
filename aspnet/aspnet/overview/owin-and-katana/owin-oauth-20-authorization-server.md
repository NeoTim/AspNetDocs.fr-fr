---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 Serveur d’autorisation (fr) Microsoft Docs
author: hongyes
description: Ce tutoriel vous guidera sur la façon de mettre en œuvre un serveur d’autorisation OAuth 2.0 en utilisant OWIN OAuth middleware. Il s’agit d’un tutoriel avancé qui ne fait que souligner ...
ms.author: riande
ms.date: 03/20/2014
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: d758fa2639d10e1b7be8d87c5d1f7adb7e292ac7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540394"
---
<a name="owin-oauth-20-authorization-server"></a>Serveur d’autorisation OAuth 2.0 OWIN
====================
par [Hongye Sun](https://github.com/hongyes) et [Praburaj Thiagarajan](https://github.com/Praburaj)

> Ce tutoriel vous guidera sur la façon de mettre en œuvre un serveur d’autorisation OAuth 2.0 en utilisant OWIN OAuth middleware. Il s’agit d’un tutoriel avancé qui ne décrit que les étapes pour créer un serveur d’autorisation OWIN OAuth 2.0. Ce n’est pas un tutoriel étape par étape. [Téléchargez le code de l’échantillon](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
>
> > [!NOTE]
> > Ce contour ne doit pas être destiné à être utilisé pour créer une application de production sécurisée. Ce tutoriel est destiné à fournir seulement un aperçu sur la façon de mettre en œuvre un serveur d’autorisation OAuth 2.0 en utilisant OWIN OAuth middleware.
>
>
> ## <a name="software-versions"></a>Versions logicielles
>
> | **Présenté dans le tutoriel** | **Travaille également avec** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) | [Visual Studio 2013 Express pour Desktop](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express). Visual Studio 2012 avec la dernière mise à jour devrait fonctionner, mais le tutoriel n’a pas été testé avec elle, et certaines sélections de menu et des boîtes de dialogue sont différentes. |
> | .NET 4.5 |  |
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster au [projet Katana sur GitHub](https://github.com/aspnet/AspNetKatana/). Pour les questions et les commentaires concernant le tutoriel lui-même, voir la section commentaires au bas de la page.


Le [cadre OAuth 2.0](http://tools.ietf.org/html/rfc6749) permet à une application tierce d’obtenir un accès limité à un service HTTP. Au lieu d’utiliser les informations d’identification du propriétaire de la ressource pour accéder à une ressource protégée, le client obtient un jeton d’accès (qui est une chaîne indiquant une portée spécifique, la durée de vie et d’autres attributs d’accès). Les jetons d’accès sont délivrés à des clients tiers par un serveur d’autorisation avec l’approbation du propriétaire de la ressource.

Ce tutoriel couvrira:

- Comment créer un serveur d’autorisation pour prendre en charge quatre types de subventions d’autorisation et rafraîchir les jetons :
    - Subvention de code d’autorisation
    - Subvention implicite
    - Subvention d’identification de mot de passe du propriétaire des ressources
    - Subvention pour les titres de compétences des clients
- Création d’un serveur de ressources qui est protégé par un jeton d’accès.
- Création de clients OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prérequis

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) ou le [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)gratuit , comme indiqué dans **Software Versions** en haut de la page.
- Familiarité avec OWIN. Voir [Getting Started avec le projet Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) et ce qui est nouveau dans [OWIN et Katana](index.md).
- Familiarité avec la terminologie [OAuth,](http://tools.ietf.org/html/rfc6749) y compris [les rôles](http://tools.ietf.org/html/rfc6749#section-1.1), [flux de protocole](http://tools.ietf.org/html/rfc6749#section-1.2), et la subvention [d’autorisation](http://tools.ietf.org/html/rfc6749#section-1.3). [L’introduction OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) offre une bonne introduction.

## <a name="create-an-authorization-server"></a>Créer un serveur d’autorisation

Dans ce tutoriel, nous allons grossièrement esquisser comment utiliser [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) et ASP.NET MVC pour créer un serveur d’autorisation. Nous espérons bientôt fournir un téléchargement pour l’échantillon terminé, car ce tutoriel n’inclut pas chaque étape. Tout d’abord, créez une application web vide nommée *AuthorizationServer* et installez les paquets suivants :

- Microsoft.AspNet.Mvc (en anglais seulement)
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (Ou tout autre paquet de connexion sociale comme Microsoft.Owin.Security.Facebook)

Ajoutez une [classe OWIN Startup](owin-startup-class-detection.md) dans le cadre du dossier racine du projet nommé *Startup*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Créez un dossier *\_App Start.* Sélectionnez le dossier *App\_Start* et utilisez Shift-Alt-A pour ajouter la version téléchargée du fichier *AuthorizationServer-App\_Start-Startup.Auth.cs.*

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Le code ci-dessus permet l’application / signe externe dans les cookies et l’authentification de Google, qui sont utilisés par le serveur d’autorisation lui-même pour gérer les comptes.

La `UseOAuthAuthorizationServer` méthode d’extension consiste à configurer le serveur d’autorisation. Les options de configuration sont les suivantes:

- `AuthorizeEndpointPath`: Le chemin de demande où les applications client redirigeront l’utilisateur-agent afin d’obtenir le consentement des utilisateurs à émettre un jeton ou un code. Il doit commencer par une barre`/Authorize`oblique de premier plan, par exemple, ".
- `TokenEndpointPath`: Les demandes de la clientèle sur le chemin de demande communiquent directement pour obtenir le jeton d’accès. Il doit commencer par une barre oblique de premier plan, comme "/Token". Si le client est délivré un [secret client\_](http://tools.ietf.org/html/rfc6749#appendix-A.2), il doit être fourni à ce point de terminaison.
- `ApplicationCanDisplayErrors`: Définissez-le `true` si l’application web veut générer une `/Authorize` page d’erreur personnalisée pour les erreurs de validation du client sur le point de terminaison. Cela n’est nécessaire que pour les cas où le navigateur n’est pas redirigé vers l’application client, par exemple, lorsque le `client_id` ou `redirect_uri` sont incorrects. Le `/Authorize` point final devrait s’attendre à voir le "oauth. Erreur", "oauth. ErreurDescription», et "oauth. Les propriétés ErrorUri" sont ajoutées à l’environnement OWIN.

    > [!NOTE]
    > Si ce n’est pas vrai, le serveur d’autorisation retournera une page d’erreur par défaut avec les détails de l’erreur.
- `AllowInsecureHttp`: Fidèle pour permettre aux demandes d’autorisation et de jetons `redirect_uri` d’arriver sur les adresses HTTP URI, et de permettre aux paramètres de demande d’autorisation entrant d’avoir des adresses HTTP URI.

    > [!WARNING]
    > Sécurité - C’est uniquement pour le développement.
- `Provider`: L’objet fourni par la demande pour traiter les événements soulevés par le middleware Authorization Server. L’application peut implémenter l’interface `OAuthAuthorizationServerProvider` entièrement, ou elle peut créer une instance et affecter les délégués nécessaires pour les flux OAuth de ce serveur prend en charge.
- `AuthorizationCodeProvider`: Produit un code d’autorisation à usage unique pour revenir à l’application client. Pour que le serveur OAuth **MUST** soit sécurisé, `AuthorizationCodeProvider` l’application DOIT `OnCreate/OnCreateAsync` fournir un exemple pour lequel `OnReceive/OnReceiveAsync`le jeton produit par l’événement est considéré comme valide pour un seul appel à .
- `RefreshTokenProvider`: Produit un jeton de rafraîchissement qui peut être utilisé pour produire un nouveau jeton d’accès en cas de besoin. Si ce n’est pas fourni le serveur `/Token` d’autorisation ne retournera pas les jetons de rafraîchissement à partir du point de terminaison.

## <a name="account-management"></a>Gestion de compte

OAuth ne se soucie pas où et comment vous gérez les informations de votre compte utilisateur. C’est [ASP.NET Identité](../../../identity/index.md) qui en est responsable. Dans ce tutoriel, nous allons simplifier le code de gestion de compte et assurez-vous que l’utilisateur peut se connecter à l’aide de cookie middleware OWIN. Voici le code d’échantillon simplifié pour le `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri`est utilisé pour valider le client avec son URL de redirection enregistrée. `ValidateClientAuthentication`vérifie l’en-tête du schéma de base et le corps de forme pour obtenir les informations d’identification du client.

La page de connexion est affichée ci-dessous :

![](owin-oauth-20-authorization-server/_static/image1.png)

Examinez maintenant la section des subventions au [Code d’autorisation](http://tools.ietf.org/html/rfc6749#section-4.1) de l’UET 2.

**Fournisseur** (dans le tableau ci-dessous) est [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Fournisseur, qui est `OAuthAuthorizationServerProvider`de type , qui contient tous les événements serveur OAuth.

| Étapes de flux de la section de subvention de code d’autorisation | Le téléchargement d’échantillons effectue ces étapes avec : |
| --- | --- |
|  |  |
| (A) Le client initie le flux en orientant l’agent d’utilisateur du propriétaire de la ressource vers le critère d’autorisation. Le client comprend son identifiant client, la portée demandée, l’état local, et une redirection URI à laquelle le serveur d’autorisation enverra l’agent utilisateur en arrière une fois l’accès est accordé (ou refusé). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) Le serveur d’autorisation authentifie le propriétaire de la ressource (par l’intermédiaire de l’agent utilisateur) et établit si le propriétaire de la ressource accorde ou refuse la demande d’accès du client. | **Si l’utilisateur accorde l’accès&gt; &lt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) En supposant que le propriétaire de la ressource accorde l’accès, le serveur d’autorisation redirige l’agent d’utilisation vers le client en utilisant la redirection URI fournie plus tôt (dans la demande ou lors de l’enregistrement du client). ... |  |
|  |  |
| (D) Le client demande un jeton d’accès à partir du critère de fin symbolique du serveur d’autorisation en incluant le code d’autorisation reçu dans l’étape précédente. Lors de la demande, le client authentifie avec le serveur d’autorisation. Le client comprend la redirection URI utilisée pour obtenir le code d’autorisation pour la vérification. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Une mise `AuthorizationCodeProvider.CreateAsync` en `ReceiveAsync` œuvre d’échantillons pour et pour contrôler la création et la validation du code d’autorisation est indiquée ci-dessous.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Le code ci-dessus utilise un dictionnaire simultané dans la mémoire pour stocker le code et le billet d’identité et restaurer l’identité après avoir reçu le code. Dans une application réelle, il serait remplacé par un magasin de données persistant. Le critère d’autorisation est pour le propriétaire de la ressource d’accorder l’accès au client. Habituellement, il a besoin d’une interface utilisateur pour permettre à l’utilisateur de cliquer sur un bouton et de confirmer la subvention. OWIN OAuth middleware permet au code d’application de gérer le critère d’autorisation. Dans notre application d’exemple, nous `OAuthController` utilisons un contrôleur MVC appelé pour le gérer. Voici l’exemple de mise en œuvre :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

L’action `Authorize` vérifiera d’abord si l’utilisateur s’est connecté au serveur d’autorisation. Si ce n’est pas le cas, le middleware d’authentification met l’appelant authentifier à l’aide du cookie "Application" et redirige vers la page de connexion. (Voir le code mis en évidence ci-dessus.) Si l’utilisateur s’est connecté, il rendra la vue d’autorisation, comme indiqué ci-dessous :

![](owin-oauth-20-authorization-server/_static/image2.png)

Si le bouton **Grant** `Authorize` est sélectionné, l’action créera une nouvelle identité « Porteur » et vous connectera avec elle. Il déclenchera le serveur d’autorisation pour générer un jeton porteur et le renvoyer au client avec la charge utile JSON.

### <a name="implicit-grant"></a>Subvention implicite

Faites maintenant référence à la section OAuth 2 [Implicit Grant](http://tools.ietf.org/html/rfc6749#section-4.2) de l’IETF.

 Le flux [implicite de subvention](http://tools.ietf.org/html/rfc6749#section-4.2) indiqué dans la figure 4 est le flux et la cartographie que le middleware OWIN OAuth suit.

| Étapes de flux de la section Subvention implicite | Le téléchargement d’échantillons effectue ces étapes avec : |
| --- | --- |
|  |  |
| (A) Le client initie le flux en orientant l’agent d’utilisateur du propriétaire de la ressource vers le critère d’autorisation. Le client comprend son identifiant client, la portée demandée, l’état local, et une redirection URI à laquelle le serveur d’autorisation enverra l’agent utilisateur en arrière une fois l’accès est accordé (ou refusé). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) Le serveur d’autorisation authentifie le propriétaire de la ressource (par l’intermédiaire de l’agent utilisateur) et établit si le propriétaire de la ressource accorde ou refuse la demande d’accès du client. | **Si l’utilisateur accorde l’accès&gt; &lt;** Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) En supposant que le propriétaire de la ressource accorde l’accès, le serveur d’autorisation redirige l’agent d’utilisation vers le client en utilisant la redirection URI fournie plus tôt (dans la demande ou lors de l’enregistrement du client). ... |  |
|  |  |
| (D) Le client demande un jeton d’accès à partir du critère de fin symbolique du serveur d’autorisation en incluant le code d’autorisation reçu dans l’étape précédente. Lors de la demande, le client authentifie avec le serveur d’autorisation. Le client comprend la redirection URI utilisée pour obtenir le code d’autorisation pour la vérification. |  |

Étant donné que nous avons`OAuthController.Authorize` déjà mis en œuvre le critère d’autorisation (action) pour la subvention de code d’autorisation, il permet automatiquement le flux implicite ainsi. Remarque `Provider.ValidateClientRedirectUri` : est utilisé pour valider l’ID du client avec son URL de redirection, qui protège le flux implicite de subvention de l’envoi du jeton d’accès à des clients malveillants ([attaque man-in-the-middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Subvention d’identification de mot de passe du propriétaire des ressources

Consultez maintenant la section subvention de la subvention de la subvention de référence des [certificats d’identification des propriétaires de ressources](http://tools.ietf.org/html/rfc6749#section-4.3) de l’IETF 2.

 Le flux [de subvention de subvention de mot de passe de propriétaire de ressource](http://tools.ietf.org/html/rfc6749#section-4.3) indiqué dans la figure 5 est le flux et la cartographie que le middleware OWIN OAuth suit.

| Étapes de flux de la section subvention de la subvention de mot de passe de propriétaire de ressource | Le téléchargement d’échantillons effectue ces étapes avec : |
| --- | --- |
|  |  |
| (A) Le propriétaire de la ressource fournit au client son nom d’utilisateur et son mot de passe. |  |
|  |  |
| (B) Le client demande un jeton d’accès à partir du point de terminaison symbolique du serveur d’autorisation en incluant les informations d’identification reçues du propriétaire de la ressource. Lors de la demande, le client authentifie avec le serveur d’autorisation. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) Le serveur d’autorisation authentifie le client et valide les informations d’identification du propriétaire de la ressource et, si elle est valide, émet un jeton d’accès. |  |

Voici l’exemple `Provider.GrantResourceOwnerCredentials`de mise en œuvre pour :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Le code ci-dessus est destiné à expliquer cette section du tutoriel et ne doit pas être utilisé dans les applications sécurisées ou de production. Il ne vérifie pas les informations d’identification des propriétaires de ressources. Il suppose que chaque titre de compétence est valide et crée une nouvelle identité pour elle. La nouvelle identité sera utilisée pour générer le jeton d’accès et rafraîchir le jeton. Veuillez remplacer le code par votre propre code de gestion de compte sécurisé.


### <a name="client-credentials-grant"></a>Subvention pour les titres de compétences des clients

Consultez maintenant la section subvention pour les [titres de compétences des](http://tools.ietf.org/html/rfc6749#section-4.4) clients de l’IETF 2.

 Le flux [de subvention pour les titres de compétences](http://tools.ietf.org/html/rfc6749#section-4.4) des clients indiqué dans la figure 6 est le flux et la cartographie que le middleware OWIN OAuth suit.

| Étapes de flux de la section Subvention pour les titres de compétences des clients | Le téléchargement d’échantillons effectue ces étapes avec : |
| --- | --- |
|  |  |
| (A) Le client authentifie avec le serveur d’autorisation et demande un signe d’accès à partir du point de terminaison symbolique. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) Le serveur d’autorisation authentifie le client et, le cas échéant, émet un signe d’accès. |  |

Voici l’exemple `Provider.GrantClientCredentials`de mise en œuvre pour :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Le code ci-dessus est destiné à expliquer cette section du tutoriel et ne doit pas être utilisé dans les applications sécurisées ou de production. Veuillez remplacer le code par votre propre code de gestion client sécurisé.


### <a name="refresh-token"></a>Actualiser le jeton

Faites référence à la section Jet 2 [Refresh Token](http://tools.ietf.org/html/rfc6749#section-1.5) de l’IETF dès maintenant.

 Le flux [de jet de jet de rafraîchissement](http://tools.ietf.org/html/rfc6749#section-1.5) indiqué dans la figure 2 est le flux et la cartographie que le middleware OWIN OAuth suit.

| Étapes de flux de la section Subvention pour les titres de compétences des clients | Le téléchargement d’échantillons effectue ces étapes avec : |
| --- | --- |
|  |  |
| (G) Le client demande un nouveau jeton d’accès en authentifiant avec le serveur d’autorisation et en présentant le jeton de rafraîchissement. Les exigences d’authentification du client sont basées sur le type de client et sur les stratégies du serveur d’autorisation. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) Le serveur d’autorisation authentifie le client et valide le jeton de rafraîchissement, et si elle est valide, émet un nouveau jeton d’accès (et, optionnellement, un nouveau jeton de rafraîchissement). |  |

Voici l’exemple `Provider.GrantRefreshToken`de mise en œuvre pour :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Créer un serveur de ressources protégé par Access Token

Créez un projet d’application web vide et installez les paquets suivants dans le projet :

- Microsoft.AspNet.WebApi.Owin (en anglais seulement)
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Créez une classe de démarrage et configurez l’authentification et l’API Web. Voir *AuthorizationServer-ResourceServer-Startup.cs* dans le téléchargement de l’échantillon.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Voir *AuthorizationServer-ResourceServer-App\_Start-Startup.Auth.cs* dans le téléchargement de l’échantillon.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Voir *AuthorizationServer-ResourceServer-App\_Start-Startup.WebApi.cs* dans le téléchargement de l’échantillon.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors`permet CORS pour tous les domaines.
- `UseOAuthBearerAuthentication`la méthode permet à oAuth porte-à-porter d’authentification middleware qui recevra et validera le signeur de l’en-tête d’autorisation dans la demande.
- `Config.SuppressDefaultHostAuthenticaiton`supprime l’hôte par défaut authentifié principal de l’application, donc toutes les demandes seront anonymes après cet appel.
- `HostAuthenticationFilter`permet l’authentification uniquement pour le type d’authentification spécifié. Dans ce cas, c’est le type d’authentification du porteur.

Afin de démontrer l’identité authentifiée, nous créons un ApiController pour obtenir les revendications actuelles de l’utilisateur.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Si le serveur d’autorisation et le serveur de ressources ne sont pas sur le même ordinateur, le middleware OAuth utilisera les différentes clés de la machine pour crypter et déchiffrer le jeton d’accès au porteur. Afin de partager la même clé privée entre `machinekey` les deux projets, nous ajoutons le même paramètre dans les deux fichiers *web.config.*

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Créer des clients OAuth 2.0

 Nous utilisons le package [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet pour simplifier le code client.

### <a name="authorization-code-grant-client"></a>Client de subvention de code d’autorisation

 Ce client est une application MVC. Il déclenchera un flux de subvention de code d’autorisation pour obtenir le jeton d’accès de backend. Il a une seule page comme indiqué ci-dessous:

![](owin-oauth-20-authorization-server/_static/image3.png)

- Le bouton **Autoriser** redirigera le navigateur vers le serveur d’autorisation pour aviser le propriétaire de la ressource d’accorder l’accès à ce client.
- Le bouton **Refresh** recevra un nouveau jeton d’accès et rafraîchira le jeton à l’aide du jeton de rafraîchissement actuel.
- Le bouton **Access Protected Resource API** appellera le serveur de ressources pour obtenir les données de réclamation de l’utilisateur actuel et les afficher sur la page.

Voici l’exemple de `HomeController` code du client.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth`exige SSL par défaut. Puisque notre démo utilise HTTP, vous devez ajouter le paramètre suivant dans le fichier config :

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Sécurité - Ne désactivez jamais SSL dans une application de production. Vos identifiants de connexion sont maintenant envoyés en texte clair sur le fil. Le code ci-dessus est juste pour le débogage d’échantillons locaux et l’exploration.


### <a name="implicit-grant-client"></a>Client implicite de subvention

Ce client utilise JavaScript pour :

1. Ouvrez une nouvelle fenêtre et rediriger vers le point de terminaison autorisé du serveur d’autorisation.
2. Obtenez le jeton d’accès à partir de fragments d’URL lorsqu’il redirige.

L’image suivante montre ce processus :

![](owin-oauth-20-authorization-server/_static/image4.png)

Le client doit avoir deux pages : l’une pour la page d’accueil et l’autre pour rappel. Voici l’exemple de code JavaScript trouvé dans le fichier *Index.cshtml:*

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Voici le code de traitement de rappel dans le fichier *SignIn.cshtml* :

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Une meilleure pratique est de déplacer le JavaScript vers un fichier externe et ne pas l’intégrer avec la balisage Razor. Pour garder cet échantillon simple, ils ont été combinés.


### <a name="resource-owner-password-credentials-grant-client"></a>Ressources Propriétaire d’informations d’identification de mot de passe Grant Client

Nous avons recours à une application console pour démor égarder ce client. Voici le code :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Client Credentials Grant Client

Semblable à la subvention d’identification de mot de passe de propriétaire de ressource, voici le code d’application de console :

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
