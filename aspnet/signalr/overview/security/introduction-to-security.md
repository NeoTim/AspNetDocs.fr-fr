---
uid: signalr/overview/security/introduction-to-security
title: Introduction à la sécurité signalR (fr) Microsoft Docs
author: bradygaster
description: Décrivez les problèmes de sécurité que vous devez prendre en considération lors de l’élaboration d’une application SignalR.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 24ce20b45543468de28ad017ba62d2f6e5a00f3b
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676194"
---
# <a name="introduction-to-signalr-security"></a>Introduction à la sécurité de SignalR

par [Patrick Fletcher](https://github.com/pfletcher), Tom [FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cet article décrit les problèmes de sécurité que vous devez prendre en considération lors du développement d’une application SignalR.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans ce sujet
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Version SignalR 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de ce sujet
>
> Pour plus d’informations sur les versions antérieures de SignalR, voir [SignalR Older Versions](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer dans les commentaires au bas de la page. Si vous avez des questions qui ne sont pas directement liées au tutoriel, vous pouvez les poster sur le [ASP.NET Forum SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Vue d'ensemble

Ce document contient les sections suivantes :

- [Concepts de sécurité SignalR](#concepts)

    - [Authentification et autorisation](#authentication)
    - [Signe de connexion](#connectiontoken)
    - [Rejoindre des groupes lors de la reconnexion](#rejoingroup)
- [Comment SignalR empêche la contrefaçon de demande de cross-site](#csrf)
- [Recommandations de sécurité SignalR](#recommendations)

    - [Protocole sécurisé de couches de douille (SSL)](#ssl)
    - [N’utilisez pas les groupes comme mécanisme de sécurité](#groupsecurity)
    - [Gérer en toute sécurité les commentaires des clients](#input)
    - [Concilier un changement de statut de l’utilisateur avec une connexion active](#reconcile)
    - [Fichiers proxy JavaScript générés automatiquement](#autogen)
    - [Exceptions](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Concepts de sécurité SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Authentification et autorisation

SignalR ne fournit aucune fonctionnalité pour authentifier les utilisateurs. Au lieu de cela, vous intégrez les fonctionnalités SignalR dans la structure d’authentification existante pour une application. Vous authentifiez les utilisateurs comme vous le feriez normalement dans votre application, et travaillez avec les résultats de l’authentification dans votre code SignalR. Par exemple, vous pouvez authentifier vos utilisateurs avec ASP.NET forme l’authentification, puis dans votre hub, appliquer quels utilisateurs ou rôles sont autorisés à appeler une méthode. Dans votre hub, vous pouvez également transmettre au client des informations d’authentification, telles que le nom d’utilisateur ou l’utilisateur.

SignalR fournit [l’attribut d’autorisation](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) pour spécifier quels utilisateurs ont accès à un hub ou à une méthode. Vous appliquez l’attribut d’autorisation à un moyeu ou à des méthodes particulières dans un hub. Sans l’attribut Autoriser, toutes les méthodes publiques sur le hub sont disponibles pour un client connecté au hub. Pour plus d’informations sur les hubs, voir [Authentification et autorisation pour les hubs SignalR](hub-authorization.md).

Vous appliquez l’attribut `Authorize` aux hubs, mais pas aux connexions persistantes. Pour faire respecter les `PersistentConnection` règles d’autorisation lors de l’utilisation d’un, vous devez passer outre à la `AuthorizeRequest` méthode. Pour plus d’informations sur les connexions persistantes, voir [Authentification et autorisation pour signalr connexions persistantes](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Signe de connexion

SignalR atténue le risque d’exécution de commandes malveillantes en validant l’identité de l’expéditeur. Pour chaque demande, le client et le serveur passent un jeton de connexion qui contient l’id de connexion et le nom d’utilisateur pour les utilisateurs authentifiés. L’id de connexion identifie de manière unique chaque client connecté. Le serveur génère au hasard l’id de connexion lorsqu’une nouvelle connexion est créée, et persiste que l’id pour la durée de la connexion. Le mécanisme d’authentification de l’application Web fournit le nom d’utilisateur. SignalR utilise le chiffrement et une signature numérique pour protéger le jeton de connexion.

![](introduction-to-security/_static/image2.png)

Pour chaque demande, le serveur valide le contenu du jeton pour s’assurer que la demande provient de l’utilisateur spécifié. Le nom d’utilisateur doit correspondre à l’id de connexion. En validant à la fois l’id de connexion et le nom d’utilisateur, SignalR empêche un utilisateur malveillant d’usurper facilement l’identité d’un autre utilisateur. Si le serveur ne peut pas valider le jeton de connexion, la demande échoue.

![](introduction-to-security/_static/image4.png)

Étant donné que l’id de connexion fait partie du processus de vérification, vous ne devez pas révéler l’identifiant de connexion d’un utilisateur à d’autres utilisateurs ou stocker la valeur sur le client, comme dans un cookie.

#### <a name="connection-tokens-vs-other-token-types"></a>Jetons de connexion par rapport à d’autres types de jetons

Les jetons de connexion sont parfois signalés par des outils de sécurité parce qu’ils semblent être des jetons de session ou des jetons d’authentification, ce qui pose un risque s’il est exposé.

Le jeton de connexion de SignalR n’est pas un jeton d’authentification. Il est utilisé pour confirmer que l’utilisateur qui fait cette demande est le même qui a créé la connexion. Le jeton de connexion est nécessaire car ASP.NET SignalR permet aux connexions de se déplacer entre les serveurs. Le jeton associe la connexion à un utilisateur particulier mais n’affirme pas l’identité de l’utilisateur qui fait la demande. Pour qu’une demande De SignalR soit correctement authentifiée, elle doit avoir un autre jeton qui affirme l’identité de l’utilisateur, comme un cookie ou un jeton porteur. Toutefois, le jeton de connexion lui-même ne prétend pas que la demande a été faite par cet utilisateur, seulement que l’ID de connexion contenu dans le jeton est associé à cet utilisateur.

Étant donné que le jeton de connexion ne fournit aucune réclamation d’authentification de ses propres, il n’est pas considéré comme un «session» ou «authentification» jeton. La prise du jeton de connexion d’un utilisateur donné et sa relecture dans une demande authentifiée comme un autre utilisateur (ou une demande non athénée) échoueront, car l’identité de l’utilisateur de la demande et l’identité stockée dans le jeton ne correspondront pas.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Rejoindre des groupes lors de la reconnexion

Par défaut, l’application SignalR réaffecte automatiquement un utilisateur aux groupes appropriés lors de la reconnexion d’une perturbation temporaire, par exemple lorsqu’une connexion est supprimée et rétablie avant la sortie des temps de connexion. Lors de la reconnexion, le client passe un jeton de groupe qui comprend l’id de connexion et les groupes assignés. Le jeton du groupe est signé numériquement et crypté. Le client conserve la même connexion id après une reconnexion; par conséquent, l’id de connexion passé par le client reconnecté doit correspondre à l’id de connexion précédent utilisé par le client. Cette vérification empêche un utilisateur malveillant de transmettre des demandes de rejoindre des groupes non autorisés lors de la reconnexion.

Cependant, il est important de noter, que le jeton de groupe n’expire pas. Si un utilisateur appartenait à un groupe dans le passé, mais a été banni de ce groupe, cet utilisateur peut être en mesure d’imiter un jeton de groupe qui comprend le groupe interdit. Si vous devez gérer en toute sécurité les utilisateurs auxquels appartiennent les groupes, vous devez stocker ces données sur le serveur, comme dans une base de données. Ensuite, ajoutez de la logique à votre application qui vérifie sur le serveur si un utilisateur appartient à un groupe. Pour un exemple de vérification des membres du groupe, voir [Travailler avec les groupes](../guide-to-the-api/working-with-groups.md).

La jointement automatique des groupes ne s’applique que lorsqu’une connexion est reconnectée après une interruption temporaire. Si un utilisateur se déconnecte en s’éloignant de l’application ou si l’application redémarre, votre application doit gérer la façon d’ajouter cet utilisateur aux groupes corrects. Pour plus d’informations, voir [Travailler avec les groupes](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Comment SignalR empêche la contrefaçon de demande de cross-site

Cross-Site Request Forgery (CSRF) est une attaque où un site malveillant envoie une demande à un site vulnérable où l’utilisateur est actuellement connecté. SignalR empêche CSRF en rendant extrêmement peu probable pour un site malveillant de créer une demande valide pour votre application SignalR.

### <a name="description-of-csrf-attack"></a>Description de l’attaque du CSRF

Voici un exemple d’attaque du CSRF :

1. Un utilisateur se connecte à www.example.com, en utilisant l’authentification des formulaires.
2. Le serveur authentifie l’utilisateur. La réponse du serveur inclut un cookie d’authentification.
3. Sans vous connecter, l’utilisateur visite un site Web malveillant. Ce site malveillant contient le formulaire HTML suivant :

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Notez que l’action de formulaire affiche le site vulnérable, pas au site malveillant. Il s’agit de la partie « cross-site » du CSRF.
4. L’utilisateur clique sur le bouton soumettre. Le navigateur inclut le cookie d’authentification avec la demande.
5. La demande s’exécute sur le serveur example.com avec le contexte d’authentification de l’utilisateur, et peut faire tout ce qu’un utilisateur authentifié est autorisé à faire.

Bien que cet exemple exige de l’utilisateur de cliquer sur le bouton de formulaire, la page malveillante peut tout aussi bien exécuter un script qui envoie une demande AJAX à votre application SignalR. En outre, l’utilisation de SSL n’empêche pas une attaque CSRF, parce que le site malveillant peut envoyer une demande de "https://".

En règle générale, les attaques CSRF sont possibles contre les sites Web qui utilisent des cookies pour l’authentification, parce que les navigateurs envoient tous les cookies pertinents sur le site Web de destination. Cependant, les attaques du CSRF ne se limitent pas à exploiter les cookies. Par exemple, l’authentification Basic et Digest est également vulnérable. Après qu’un utilisateur se connecte avec l’authentification Basic ou Digest, le navigateur envoie automatiquement les informations d’identification jusqu’à la fin de la session.

### <a name="csrf-mitigations-taken-by-signalr"></a>Atténuations CSRF prises par SignalR

SignalR prend les mesures suivantes pour empêcher un site malveillant de créer des demandes valides à votre application. SignalR prend ces mesures par défaut, vous n’avez pas besoin de prendre aucune mesure dans votre code.

- **Désactiver les demandes de domaine transversal** SignalR désactive les demandes de domaine croisé pour empêcher les utilisateurs d’appeler un point de terminaison SignalR à partir d’un domaine externe. SignalR considère que toute demande d’un domaine externe est invalide et bloque la demande. Nous vous recommandons de conserver ce comportement par défaut; autrement, un site malveillant pourrait tromper les utilisateurs en envoyant des commandes à votre site. Si vous avez besoin d’utiliser des demandes de domaine transversal, voir [comment établir une connexion cross-domain](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Passer le jeton de connexion dans la chaîne de requête, pas le cookie** SignalR passe le jeton de connexion comme une valeur de chaîne de requête, au lieu de comme un cookie. Stocker le jeton de connexion dans un cookie est dangereux parce que le navigateur peut par inadvertance transmettre le jeton de connexion lorsque du code malveillant est rencontré. En outre, le passage du jeton de connexion dans la chaîne de requête empêche le jeton de connexion de persister au-delà de la connexion actuelle. Par conséquent, un utilisateur malveillant ne peut pas faire une demande en vertu des informations d’authentification d’un autre utilisateur.
- **Vérifier le jeton de connexion** Tel que décrit dans la section [Signeton de connexion,](#connectiontoken) le serveur sait quel identifiant de connexion est associé à chaque utilisateur authentifié. Le serveur ne traite aucune demande à partir d’un identifiant de connexion qui ne correspond pas au nom d’utilisateur. Il est peu probable qu’un utilisateur malveillant puisse deviner une demande valide parce que l’utilisateur malveillant devrait connaître le nom d’utilisateur et l’id de connexion généré au hasard actuel. Cette pièce d’identité de connexion devient invalide dès que la connexion est terminée. Les utilisateurs anonymes ne devraient pas avoir accès à des informations sensibles.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recommandations de sécurité SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocole sécurisé de couches de douille (SSL)

Le protocole SSL utilise le chiffrement pour sécuriser le transport de données entre un client et un serveur. Si votre application SignalR transmet des informations sensibles entre le client et le serveur, utilisez SSL pour le transport. Pour plus d’informations sur la mise en place de SSL, voir [Comment configurer SSL sur IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>N’utilisez pas les groupes comme mécanisme de sécurité

Les groupes sont un moyen pratique de recueillir les utilisateurs apparentés, mais ils ne sont pas un mécanisme sécurisé pour limiter l’accès aux informations sensibles. Cela est particulièrement vrai lorsque les utilisateurs peuvent automatiquement rejoindre des groupes lors d’une reconnexion. Au lieu de cela, envisagez d’ajouter des utilisateurs privilégiés à un rôle et de limiter l’accès à une méthode de plaque tournante pour les seuls membres de ce rôle. Pour un exemple de restriction de l’accès basé sur un rôle, voir [Authentification et autorisation pour les hubs SignalR](hub-authorization.md). Par exemple, vérifier l’accès des utilisateurs aux groupes lors de la reconnexion, voir [Travailler avec les groupes](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Gérer en toute sécurité les commentaires des clients

Pour vous assurer qu’un utilisateur malveillant n’envoie pas de script à d’autres utilisateurs, vous devez coder toutes les entrées des clients destinés à être diffusées à d’autres clients. Vous devez coder des messages sur les clients récepteurs plutôt que sur le serveur, car votre application SignalR peut avoir de nombreux types de clients différents. Par conséquent, l’encodage HTML fonctionne pour un client web, mais pas pour d’autres types de clients. Par exemple, une méthode de client Web pour afficher un message de `html()` chat gérerait en toute sécurité le nom d’utilisateur et le message en appelant la fonction.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Concilier un changement de statut de l’utilisateur avec une connexion active

Si le statut d’authentification d’un utilisateur change pendant l’existence d’une connexion active, l’utilisateur recevra une erreur qui indique : « L’identité utilisateur ne peut pas changer lors d’une connexion SignalR active. » Dans ce cas, votre application doit se re-connecter au serveur pour s’assurer que l’id de connexion et le nom d’utilisateur sont coordonnés. Par exemple, si votre application permet à l’utilisateur de se déconnecter pendant qu’une connexion active existe, le nom d’utilisateur de la connexion ne correspondra plus au nom qui est transmis pour la prochaine demande. Vous voudrez arrêter la connexion avant que l’utilisateur se connecte, puis le redémarrer.

Cependant, il est important de noter que la plupart des applications n’auront pas besoin de s’arrêter manuellement et de démarrer la connexion. Si votre application redirige les utilisateurs vers une page distincte après la connexion, comme le comportement par défaut dans une application Web Forms ou une application MVC, ou rafraîchit la page actuelle après la connexion, la connexion active est automatiquement déconnectée et ne nécessite aucune action supplémentaire.

L’exemple suivant montre comment arrêter et démarrer une connexion lorsque l’état de l’utilisateur a changé.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Ou, le statut d’authentification de l’utilisateur peut changer si votre site utilise l’expiration coulissante avec Forms Authentication, et il n’y a aucune activité pour garder le cookie d’authentification valide. Dans ce cas, l’utilisateur sera déconnecté et le nom d’utilisateur ne correspondra plus au nom d’utilisateur dans le jeton de connexion. Vous pouvez résoudre ce problème en ajoutant un script qui demande périodiquement une ressource sur le serveur Web pour garder le cookie d’authentification valide. L’exemple suivant montre comment demander une ressource toutes les 30 minutes.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Fichiers proxy JavaScript générés automatiquement

Si vous ne souhaitez pas inclure tous les hubs et méthodes du fichier proxy JavaScript pour chaque utilisateur, vous pouvez désactiver la génération automatique du fichier. Vous pouvez choisir cette option si vous avez plusieurs hubs et méthodes, mais ne veulent pas que chaque utilisateur soit au courant de toutes les méthodes. Vous désactivez la génération automatique en définissant **Lesproxies EnableJavaScript** à **faux**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Pour plus d’informations sur les fichiers proxy JavaScript, voir [Le proxy généré et ce qu’il fait pour vous](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Exceptions

Vous devez éviter de transmettre des objets d’exception aux clients parce que les objets peuvent exposer des informations sensibles aux clients. Au lieu de cela, appelez une méthode sur le client qui affiche le message d’erreur pertinent.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
