---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Présentation de l’authentification ASP.NET AJAX et les Services d’Application de profil | Microsoft Docs
author: scottcate
description: Le service d’authentification permet aux utilisateurs de fournir des informations d’identification afin de recevoir un cookie d’authentification et est le service de passerelle pour autoriser l’utilisateur personnalisée...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 1087d9120411e51fd61d073169a88cac6cdaf15b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65109490"
---
# <a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Présentation de l’authentification et des services d’application de profil d’ASP.NET AJAX

par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Le service d’authentification permet aux utilisateurs de fournir des informations d’identification afin de recevoir un cookie d’authentification, et est le service de passerelle pour autoriser les profils d’utilisateur personnalisée fourni par ASP.NET. Utilisation du service d’authentification ASP.NET AJAX est compatible avec l’authentification des formulaires ASP.NET standard pour les applications actuellement à l’aide de l’authentification par formulaire (par exemple, avec la connexion contrôler) ne seront pas interrompues par la mise à niveau vers le service d’authentification AJAX.

## <a name="introduction"></a>Introduction

Dans le cadre de .NET Framework 3.5, Microsoft fournit une mise à niveau de l’environnement dimensionnable ; non seulement un environnement de développement n’est disponible, mais les nouvelles fonctionnalités de Language-Integrated Query (LINQ) et d’autres améliorations de langage sont à venir. En outre, certaines fonctionnalités connues d’autres ensembles d’outils, notamment les Extensions ASP.NET AJAX, sont en cours incluses en tant que membres de première classe de la bibliothèque de classes de Base .NET Framework. Ces extensions permettent de nombreuses nouvelles fonctionnalités de client riche, notamment le rendu partiel de pages sans nécessiter une actualisation de la page entière, la possibilité d’accéder aux Services Web via le script client (y compris les API de profilage ASP.NET) et une API côté client étendue conçu pour mettre en miroir de la plupart des schémas de contrôle indiqués dans l’ensemble du contrôle côté serveur ASP.NET.

Ce livre blanc examine l’implémentation et l’utilisation du profilage ASP.NET et services de l’authentification par formulaire comme ils sont exposés par les Extensions AJAX Microsoft ASP.NET AJAX ExtensionsThe facilitent l’authentification par formulaire incroyablement prendre en charge, tel qu’il (ainsi que le Service de profilage) sont exposé via un script de proxy de Service Web. Les Extensions AJAX prennent également en charge l’authentification personnalisée via la classe AuthenticationServiceManager.

Ce livre blanc est basé sur la version bêta 2 de Visual Studio 2008 et .NET Framework 3.5. Ce livre blanc suppose également que vous allez travailler avec Visual Studio 2008 Bêta 2, pas Visual Web Developer Express et fournissez des procédures pas à pas en fonction de l’interface utilisateur de Visual Studio. Des exemples de code peuvent utiliser des modèles de projet non disponibles dans Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Profils et l’authentification*

Les profils ASP.NET de Microsoft et les services d’authentification sont fournies par le système d’authentification par formulaire ASP.NET et sont des composants standard d’ASP.NET. Les Extensions ASP.NET AJAX fournissent l’accès à ces services par le biais de proxys de script, via un modèle relativement simple sous Sys.Services espace de noms de la bibliothèque cliente AJAX de script.

Le service d’authentification permet aux utilisateurs de fournir des informations d’identification afin de recevoir un cookie d’authentification, et est le service de passerelle pour autoriser les profils d’utilisateur personnalisée fourni par ASP.NET. Utilisation du service d’authentification ASP.NET AJAX est compatible avec l’authentification des formulaires ASP.NET standard pour les applications actuellement à l’aide de l’authentification par formulaire (par exemple, avec la connexion contrôler) ne seront pas interrompues par la mise à niveau vers le service d’authentification AJAX.

Le service de profil permet l’intégration automatique et le stockage des données utilisateur selon l’appartenance au tel que fourni par le service d’authentification. Les données stockées sont spécifiées par le fichier web.config et les différents fournisseurs de services de profilage assurent la gestion de données. Comme avec le service d’authentification, le service de profil d’AJAX est compatible avec le service de profil ASP.NET standard, afin que les pages actuellement incorporation des fonctionnalités du service de profil ASP.NET ne doivent pas être divisés en incluant la prise en charge AJAX.

L’incorporation de l’authentification ASP.NET et des services de profilage eux-mêmes dans une application est en dehors de la portée de ce livre blanc. Pour plus d’informations sur le sujet, consultez MSDN Library font référence à l’article Gestion des utilisateurs à l’aide de l’adhésion à [ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET inclut également un utilitaire permettant de configurer automatiquement l’appartenance à un serveur SQL, qui est le fournisseur de service d’authentification par défaut pour l’appartenance ASP.NET. Pour plus d’informations, consultez l’article ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe) à [ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*À l’aide du Service d’authentification ASP.NET AJAX*

Le service de l’authentification ASP.NET AJAX doit être activé dans le fichier web.config :

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Le service d’authentification requiert une authentification de formulaires ASP.NET doit être activé et nécessite des cookies doit être activée sur le navigateur client (un script ne peut pas activer une session sans cookie dans la mesure où les sessions sans cookies requièrent des paramètres d’URL).

Une fois que le service d’authentification AJAX est activé et configuré, script client peut tirer parti immédiatement de l’objet Sys.Services.AuthenticationService. Principalement, script client souhaitez tirer parti de la `login` méthode et `isLoggedIn` propriété. Il existe plusieurs propriétés pour fournir des valeurs par défaut pour la méthode de connexion, ce qui peut accepter un grand nombre de paramètres.

*Membres de Sys.Services.AuthenticationService*

*méthode de connexion :*

La méthode login() commence une demande pour authentifier les informations d’identification de l’utilisateur. Cette méthode est asynchrone et ne bloque pas l’exécution.

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| userName | Obligatoire. Le nom d’utilisateur à authentifier. |
| mot de passe | Facultatif (par défaut, null). Mot de passe de l'utilisateur. |
| isPersistent | Facultatif (false par défaut). Indique si le cookie d’authentification de l’utilisateur doit persister entre les sessions. Si la valeur est false, l’utilisateur sera Déconnectez-vous une fois le navigateur est fermé ou que la session expire. |
| redirectUrl | Facultatif (par défaut, null). L’URL vers laquelle rediriger le navigateur après une authentification réussie. Si ce paramètre est null ou une chaîne vide, aucune redirection se produit. |
| customInfo | Facultatif (par défaut, null). Ce paramètre n’est actuellement pas utilisé et est réservé pour une utilisation ultérieure. |
| loginCompletedCallback | Facultatif (par défaut, null). La fonction à appeler lorsque la connexion est terminée avec succès. Si spécifié, ce paramètre remplace la propriété defaultLoginCompleted. |
| failedCallback | Facultatif (par défaut, null). La fonction à appeler lorsque la connexion a échoué. Si spécifié, ce paramètre remplace la propriété defaultFailedCallback. |
| userContext | Facultatif (par défaut, null). Données de contexte d’utilisateur personnalisées qui doivent être passées aux fonctions de rappel. |

*Valeur de retour :*

Cette fonction n’inclut pas une valeur de retour. Toutefois, un nombre de comportements est inclus à l’achèvement d’un appel à cette fonction :

- La page actuelle sera actualisée ou être modifiée si le `redirectUrl` paramètre a été ni null ni une chaîne vide.
- Toutefois, si le paramètre était null ou une chaîne vide, le `loginCompletedCallback` paramètre, ou `defaultLoginCompletedCallback` propriété est appelée.
- Si l’appel au service web échoue, le `failedCallback` paramètre de `defaultFailedCallback` propriété est appelée.

*méthode de déconnexion :*

La méthode logout() supprime le cookie d’informations d’identification et se déconnecte l’utilisateur actuel à partir de l’application web.

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| redirectUrl | Facultatif (par défaut, null). L’URL vers laquelle rediriger le navigateur après une authentification réussie. Si ce paramètre est null ou une chaîne vide, aucune redirection se produit. |
| logoutCompletedCallback | Facultatif (par défaut, null). La fonction à appeler lors de la déconnexion terminée avec succès. Si spécifié, ce paramètre remplace la propriété defaultLogoutCompleted. |
| failedCallback | Facultatif (par défaut, null). La fonction à appeler lorsque la connexion a échoué. Si spécifié, ce paramètre remplace la propriété defaultFailedCallback. |
| userContext | Facultatif (par défaut, null). Données de contexte d’utilisateur personnalisées qui doivent être passées aux fonctions de rappel. |

*Valeur de retour :*

Cette fonction n’inclut pas une valeur de retour. Toutefois, un nombre de comportements est inclus à l’achèvement d’un appel à cette fonction :

- La page actuelle sera actualisée ou être modifiée si le `redirectUrl` paramètre a été ni null ni une chaîne vide.
- Toutefois, si le paramètre était null ou une chaîne vide, le `logoutCompletedCallback` paramètre, ou `defaultLogoutCompletedCallback` propriété est appelée.
- Si l’appel au service web échoue, le `failedCallback` paramètre de `defaultFailedCallback` propriété est appelée.

*defaultFailedCallback, propriété (get, set) :*

Cette propriété spécifie une fonction qui doit être appelée en cas de défaillance pour communiquer avec le service web. Il doit recevoir un délégué (ou une référence de fonction).

La référence de la fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| error | Spécifie les informations d’erreur. |
| userContext | Spécifie les informations de contexte d’utilisateur fournies lors de la fonction de connexion ou déconnexion a été appelée. |
| methodName | Le nom de la méthode d’appel. |

*defaultLoginCompletedCallback, propriété (get, set) :*

Cette propriété spécifie une fonction qui doit être appelée lorsque l’appel de service web connexion est terminée. Il doit recevoir un délégué (ou une référence de fonction).

La référence de la fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| validCredentials | Spécifie si l’utilisateur a fourni les informations d’identification valides. `true` Si l’utilisateur connecté avec succès ; sinon `false`. |
| userContext | Spécifie les informations de contexte d’utilisateur fournies quand la fonction de connexion a été appelée. |
| methodName | Le nom de la méthode d’appel. |

*defaultLogoutCompletedCallback, propriété (get, set) :*

Cette propriété spécifie une fonction qui doit être appelée lorsque l’appel de service web déconnexion est terminé. Il doit recevoir un délégué (ou une référence de fonction).

La référence de la fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| result | Ce paramètre sera toujours `null`; elle est réservée pour une utilisation ultérieure. |
| userContext | Spécifie les informations de contexte d’utilisateur fournies quand la fonction de connexion a été appelée. |
| methodName | Le nom de la méthode d’appel. |

*isLoggedIn, propriété (get) :*

Cette propriété obtient l’état actuel de l’authentification de l’utilisateur ; Il est défini par l’objet de ScriptManager lors d’une demande de page.

Cette propriété retourne `true` si l’utilisateur est actuellement connecté ; sinon, elle retourne `false`.

*propriété de chemin d’accès (get, set) :*

Cette propriété détermine par programmation à l’emplacement du service web d’authentification. Il peut être utilisé pour remplacer le fournisseur d’authentification par défaut, mais aussi une définies de manière déclarative dans la propriété de chemin d’accès du nœud enfant du contrôle ScriptManager, AuthenticationService (pour plus d’informations, consultez l’utilisation un fournisseur de Service d’authentification personnalisé rubrique ci-dessous).

Notez que l’emplacement du service d’authentification par défaut ne change pas. Toutefois, ASP.NET AJAX vous permet de spécifier l’emplacement d’un service web qui fournit la même interface de classe en tant que le proxy du service d’authentification ASP.NET AJAX.

Notez également que cette propriété ne doit pas être définie sur une valeur qui indique à la demande de script sur le site actuel. Étant donné que l’application actuelle recevrait pas les informations d’identification d’authentification, il serait inutile ; en outre, le AJAX sous-jacent de technologie ne doit pas publier de requêtes cross-site et peut générer une exception de sécurité dans le navigateur client.

Cette propriété est un `String` objet représentant le chemin d’accès au service web d’authentification.

*propriété Timeout (get, set) :*

Cette propriété détermine la longueur du délai d’attente du service d’authentification avant de considérer que la demande de connexion a échoué. Si l’expiration du délai en attendant la fin d’un appel, le rappel échec de la demande sera appelé, et l’appel ne se terminera pas.

Cette propriété est un `Number` objet représentant le nombre de millisecondes à attendre des résultats du service d’authentification.

*Exemple de code : Journalisation dans le Service d’authentification*

Le balisage suivant est un exemple de page ASP.NET avec un appel de script simple pour les méthodes de connexion et de déconnexion de la classe Sys.Services.AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>L’accès aux données via AJAX de profilage ASP.NET

Service de profilage ASP.NET sont également exposées via les Extensions ASP.NET AJAX. Étant donné que le service de profilage ASP.NET fournit une API riche et précise, permettant de stocker et récupérer des données utilisateur, cela peut être un outil de productivité.

Le service de profil doit être activé dans le fichier web.config ; Il n’est pas par défaut. Pour ce faire, vérifiez que le `profileService` élément enfant a activé = true web.config spécifié dans, et que vous avez spécifié les propriétés qui peuvent être lues ou écrites comme suit :

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Le service de profil doit également être configuré. Configuration du service de profilage se trouve en dehors de la portée de ce livre blanc, mais il est intéressant de noter que les groupes, comme défini dans les paramètres de configuration de profil sera accessibles en tant que sous-propriétés du nom du groupe. Par exemple, avec la section suivante de profil spécifiée :

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Script client serait en mesure d’accéder nom Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip et BackgroundColor en tant que propriétés du champ de propriétés de la classe ProfileService.

Une fois que le Service de profilage AJAX est configuré, il sera immédiatement disponible dans les pages ; Toutefois, il devra être chargé qu’une seule fois avant utilisation.

*Sys.Services membres*

*champ de propriétés :*

Le champ des propriétés expose toutes les données de profil configuré en tant que propriétés enfants qui peuvent être référencées par la convention de nom de l’opérateur point. Les propriétés qui sont des groupes de propriétés enfants sont appelées GroupName.PropertyName. Dans l’exemple de configuration de profil présentée ci-dessus, pour obtenir l’état de l’utilisateur, vous pouvez utiliser l’identificateur suivant :

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*méthode Load :*

Charge une liste sélectionnée ou toutes les propriétés à partir du serveur.

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| propertyNames | Facultatif (par défaut, null). Les propriétés à charger à partir du serveur. |
| loadCompletedCallback | Facultatif (par défaut, null). La fonction à appeler lors du chargement est terminée. |
| failedCallback | Facultatif (par défaut, null). La fonction à appeler si une erreur se produit. |
| userContext | Facultatif (par défaut, null). Informations de contexte à passer à la fonction de rappel. |

La fonction de chargement n’a pas une valeur de retour. Si l’appel s’est terminée correctement, il appellera soit le `loadCompletedCallback` paramètre ou `defaultLoadCompletedCallback` propriété. Si l’appel a échoué, ou le délai d’attente a expiré, soit le `failedCallback` paramètre ou `defaultFailedCallback` propriété sera appelée.

Si le `propertyNames` paramètre n’est pas fourni, toutes les propriétés en lecture-configuré sont récupérées à partir du serveur.

*méthode Save :*

La méthode save() enregistre la liste de propriétés spécifié (ou toutes les propriétés) dans le profil d’utilisateur ASP.NET.

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| propertyNames | Facultatif (par défaut, null). Les propriétés à enregistrer dans le serveur. |
| saveCompletedCallback | Facultatif (par défaut, null). La fonction à appeler lors de l’enregistrement est terminée. |
| failedCallback | Facultatif (par défaut, null). La fonction à appeler si une erreur se produit. |
| userContext | Facultatif (par défaut, null). Informations de contexte à passer à la fonction de rappel. |

L’enregistrement fonction n’a pas une valeur de retour. Si l’appel se termine correctement, il appellera soit le `saveCompletedCallback` paramètre ou `defaultSaveCompletedCallback` propriété. Si l’appel a échoué, ou le délai d’attente a expiré, soit le `failedCallback` ou `defaultFailedCallback` propriété sera appelée.

Si le `propertyNames` paramètre est null, toutes les propriétés de profil seront envoyées au serveur, et le serveur décidera quelles propriétés peuvent être enregistrées ou non.

*defaultFailedCallback, propriété (get, set) :*

Cette propriété spécifie une fonction qui doit être appelée en cas de défaillance pour communiquer avec le service web. Il doit recevoir un délégué (ou une référence de fonction).

La référence de la fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| Error | Spécifie les informations d’erreur. |
| userContext | Spécifie les informations de contexte d’utilisateur fournies lorsque la charge ou de la fonction d’enregistrement a été appelée. |
| methodName | Le nom de la méthode d’appel. |

*defaultSaveCompleted, propriété (get, set) :*

Cette propriété spécifie une fonction qui doit être appelée à la fin de l’enregistrement des données de profil de l’utilisateur. Il doit recevoir un délégué (ou une référence de fonction).

La référence de la fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| numPropsSaved | Spécifie le nombre de propriétés qui ont été enregistrés. |
| userContext | Spécifie les informations de contexte d’utilisateur fournies lorsque la charge ou de la fonction d’enregistrement a été appelée. |
| methodName | Le nom de la méthode d’appel. |

*defaultLoadCompleted, propriété (get, set) :*

Cette propriété spécifie une fonction qui doit être appelée à la fin du chargement des données de profil de l’utilisateur. Il doit recevoir un délégué (ou une référence de fonction).

La référence de la fonction spécifiée par cette propriété doit avoir la signature suivante :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Paramètres :*

| **Nom du paramètre** | **Signification** |
| --- | --- |
| numPropsLoaded | Spécifie le nombre de propriétés chargées. |
| userContext | Spécifie les informations de contexte d’utilisateur fournies lorsque la charge ou de la fonction d’enregistrement a été appelée. |
| methodName | Le nom de la méthode d’appel. |

*propriété de chemin d’accès (get, set) :*

Cette propriété détermine par programmation à l’emplacement du service web profil. Il peut être utilisé pour remplacer le fournisseur de service de profil par défaut, mais aussi une définies de manière déclarative dans la propriété de chemin d’accès du nœud enfant du contrôle ScriptManager, ProfileService.

Notez que l’emplacement du service de profil par défaut ne change pas. Toutefois, ASP.NET AJAX vous permet de spécifier l’emplacement d’un service web qui fournit la même interface de classe en tant que le proxy du service d’authentification ASP.NET AJAX.

Notez également que cette propriété ne doit pas être définie sur une valeur qui indique à la demande de script sur le site actuel. L’AJAX de technologie sous-jacente ne doit pas publier de requêtes cross-site et peut générer une exception de sécurité dans le navigateur client.

Cette propriété est un `String` objet représentant le chemin d’accès au service web de profil.

*propriété Timeout (get, set) :*

Cette propriété détermine la durée pendant laquelle attendre pour le service de profil avant de considérer que la charge ou enregistrer la demande a échoué. Si l’expiration du délai en attendant la fin d’un appel, le rappel échec de la demande sera appelé, et l’appel ne se terminera pas.

Cette propriété est un `Number` objet représentant le nombre de millisecondes à attendre des résultats à partir du service de profil.

*Exemple de code : Chargement de données de profil au chargement de la page*

Le code suivant vérifie si un utilisateur est authentifié et si tel est le cas, chargera couleur d’arrière-plan par défaut de l’utilisateur en tant que la page.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*À l’aide d’un fournisseur de Service d’authentification personnalisé*

Les Extensions ASP.NET AJAX vous autorise à créer un fournisseur de services de l’authentification de script personnalisé en exposant vos fonctionnalités via un service web personnalisé. Pour pouvoir être utilisé, votre service web doit exposer deux méthodes, `Login` et `Logout`; et ces méthodes doivent être spécifiés avec les signatures de méthode de même que le service web de l’authentification ASP.NET AJAX par défaut.

Une fois que vous avez créé le service web personnalisé, vous devez spécifier le chemin d’accès à celui-ci, de façon déclarative sur votre page, par programmation dans le code, ou via le script client.

*Pour définir le chemin d’accès de façon déclarative :*

Pour définir le chemin d’accès de façon déclarative, incluent l’enfant AuthenticationService de l’objet de ScriptManager sur votre page ASP.NET :

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Pour définir le chemin d’accès dans le code :*

Pour définir le chemin d’accès par programme, spécifiez le chemin d’accès par le biais de l’instance de votre gestionnaire de script :

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Pour définir le chemin d’accès dans le script :*

Pour définir le chemin d’accès par programme dans le script, vous devez utiliser le `path` propriété de la classe Sys.Services.AuthenticationService :

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Exemple de Service Web pour l’authentification personnalisée*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Récapitulatif

-En particulier les services de profilage, l’appartenance et l’authentification - les services ASP.NET sont facilement exposées à JavaScript sur le navigateur client. Cela permet aux développeurs d’intégrer leur code côté client le mécanisme d’authentification en toute transparence, sans en fonction des contrôles tels qu’UpdatePanel pour faire l’essentiel. Données de profil peuvent être protégées à partir du client, en utilisant les paramètres de configuration web ; aucune donnée n’est disponible par défaut, et les développeurs doivent participer à des propriétés de profil.

En outre, en créant des implémentations de service web simplifiée avec des signatures de méthode équivalente, les développeurs peuvent créer des fournisseurs de script personnalisé pour ces services ASP.NET intrinsèques. Prise en charge de ces techniques simplifie le développement d’applications clientes riches, tout en offrant aux développeurs un large éventail de flexibilité pour répondre aux besoins spécifiques.

## <a name="bio"></a>*Bio*

Scott Cate travaille avec les technologies Web Microsoft depuis 1997 et est le président de myKB.com ([www.myKB.com](http://www.myKB.com)) où il est spécialisé dans l’écriture d’ASP.NET en fonction des applications axées sur les solutions logicielles de la Base de connaissances. Scott peut être contacté par courrier électronique en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Suivant](understanding-asp-net-ajax-localization.md)
