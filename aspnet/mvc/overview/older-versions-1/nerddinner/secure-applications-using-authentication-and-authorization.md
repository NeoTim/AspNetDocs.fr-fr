---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Applications sécurisées à l’aide de l’authentification et de l’autorisation . Microsoft Docs
author: rick-anderson
description: L’étape 9 montre comment ajouter l’authentification et l’autorisation pour sécuriser notre application NerdDinner, afin que les utilisateurs doivent s’inscrire et se connecter au site pour créer...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d96f2763f6e62f9dd599fdb7977a97993d218305
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542571"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Sécuriser des applications avec l’authentification et l’autorisation

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 9 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> Étape 9 montre comment ajouter l’authentification et l’autorisation pour sécuriser notre application NerdDinner, de sorte que les utilisateurs doivent s’inscrire et se connecter au site pour créer de nouveaux dîners, et seul l’utilisateur qui héberge un dîner peut le modifier plus tard.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner Étape 9: Authentification et autorisation

En ce moment, notre application NerdDinner accorde à toute personne visitant le site la possibilité de créer et de modifier les détails de n’importe quel dîner. Changeons ceci de sorte que les utilisateurs doivent s’inscrire et se connecter au site pour créer de nouveaux dîners, et ajouter une restriction de sorte que seul l’utilisateur qui héberge un dîner peut le modifier plus tard.

Pour ce faire, nous utiliserons l’authentification et l’autorisation pour sécuriser notre application.

### <a name="understanding-authentication-and-authorization"></a>Comprendre l’authentification et l’autorisation

*L’authentification* est le processus d’identification et de validation de l’identité d’un client accédant à une application. En d’autres termes, il s’agit d’identifier « qui » l’utilisateur final est lorsqu’il visite un site Web. ASP.NET prend en charge plusieurs façons d’authentifier les utilisateurs du navigateur. Pour les applications Web Internet, l’approche d’authentification la plus courante utilisée s’appelle « Authentification des formes ». Forms Authentication permet à un développeur d’écrire un formulaire de connexion HTML dans son application, puis de valider le nom d’utilisateur/mot de passe qu’un utilisateur final soumet à une base de données ou à un autre magasin d’informations de mot de passe. Si la combinaison nom d’utilisateur/mot de passe est correcte, le développeur peut alors demander à ASP.NET d’émettre un cookie HTTP crypté pour identifier l’utilisateur à travers les futures demandes. Nous utiliserons des formulaires d’authentification avec notre application NerdDinner.

*L’autorisation* est le processus de déterminer si un utilisateur authentifié a la permission d’accéder à une URL/ressource particulière ou d’effectuer une action. Par exemple, dans notre application NerdDinner, nous vous autoriserons que seuls les utilisateurs qui sont connectés peuvent accéder à l’URL */Dîners/Créer* et créer de nouveaux dîners. Nous voulons également ajouter la logique d’autorisation afin que seul l’utilisateur qui héberge un dîner puisse le modifier et refuser l’accès à l’édition à tous les autres utilisateurs.

### <a name="forms-authentication-and-the-accountcontroller"></a>Formulaires Authentification et le AccountController

Le modèle de projet Visual Studio par défaut pour ASP.NET MVC permet automatiquement l’authentification des formulaires lors de la création de nouvelles applications ASP.NET MVC. Il ajoute également automatiquement une mise en œuvre de la page de connexion de compte pré-construite au projet, ce qui facilite l’intégration de la sécurité dans un site.

La page principale par défaut de Site.master affiche un lien " Log On " en haut à droite du site lorsque l’utilisateur qui y accède n’est pas authentifié :

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

En cliquant sur le lien "Log On" emmène un utilisateur vers l’URL */Compte/LogOn:*

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Les visiteurs qui ne se sont pas inscrits peuvent le faire en cliquant sur le lien « Registre », qui les mènera à l’URL */Compte/Register* et leur permettra d’entrer les détails du compte :

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

En cliquant sur le bouton "Register", créer un nouvel utilisateur dans le système d’adhésion ASP.NET et authentifier l’utilisateur sur le site à l’aide de formulaires d’authentification.

Lorsqu’un utilisateur est connecté, le Site.master modifie le haut à droite de la page pour obtenir un « Bienvenue [nom d’utilisateur] ! » message et rend un lien "Log Off" au lieu d’un "Log On". En cliquant sur le lien "Log Off" connecte l’utilisateur:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La fonctionnalité de connexion ci-dessus, de logout et d’enregistrement est implémentée au sein de la classe AccountController qui a été ajoutée à notre projet par Visual Studio lorsqu’elle a créé le projet. L’interface utilisateur du AccountController est implémentée à l’aide de modèles de vue dans l’annuaire « Vues -Compte » :

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La classe AccountController utilise le système d’authentification des formulaires ASP.NET pour émettre des cookies d’authentification cryptés, et l’API ASP.NET adhésion pour stocker et valider les noms d’utilisateur/mots de passe. L’API d’adhésion ASP.NET est extensible et permet d’utiliser n’importe quel magasin d’identification de mot de passe. ASP.NET navires avec des implémentations intégrées de fournisseur d’adhésion qui stockent le nom d’utilisateur/mots de passe dans une base de données SQL, ou dans Active Directory.

Nous pouvons configurer quel fournisseur d’adhésion notre application NerdDinner devrait utiliser en ouvrant le &lt;fichier&gt; "web.config" à la racine du projet et en recherchant la section d’adhésion en son sein. Le web.config par défaut ajouté lors de la création du projet enregistre le fournisseur d’adhésion SQL, et le configure pour utiliser une chaîne de connexion nommée "ApplicationServices" pour spécifier l’emplacement de la base de données.

La chaîne de connexion par défaut "ApplicationServices" (qui est spécifiée dans la &lt;section connectionStrings&gt; du fichier web.config) est configurée pour utiliser SQL Express. Il indique une base de données SQL Express nommée "ASPNETDB. MDF» dans le cadre\_de l’annuaire «App Data» de l’application. Si cette base de données n’existe pas la première fois que l’API d’adhésion est utilisée dans l’application, ASP.NET créera automatiquement la base de données et fournira le schéma approprié de base de données d’adhésion en son sein :

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Si au lieu d’utiliser SQL Express, nous voulions utiliser une instance complète SQL Server (ou nous connecter à une base de données à distance), tout ce que nous aurions à faire est de mettre à jour la chaîne de connexion "ApplicationServices" dans le fichier web.config et de s’assurer que le schéma d’adhésion approprié a été ajouté à la base de données qu’il pointe. Vous pouvez exécuter l’utilitaire "aspnet\_regsql.exe" dans l’annuaire "Windows-Microsoft.NET-Framework"v2.0.50727" pour ajouter le schéma approprié pour l’adhésion et les autres services d’application ASP.NET à une base de données.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autoriser l’URL /Dîners/Créer à l’aide du filtre [Autoriser]

Nous n’avons pas eu à écrire de code pour permettre une authentification sécurisée et la mise en œuvre de la gestion de compte pour l’application NerdDinner. Les utilisateurs peuvent enregistrer de nouveaux comptes avec notre application, et se connecter / logout du site.

Maintenant, nous pouvons ajouter la logique d’autorisation à l’application, et utiliser le statut d’authentification et le nom d’utilisateur des visiteurs pour contrôler ce qu’ils peuvent et ne peuvent pas faire dans le site. Commençons par ajouter la logique d’autorisation aux méthodes d’action « Créer » de notre classe DinnersController. Plus précisément, nous exigerons que les utilisateurs accédant à l’URL */Dîners/Créer* soient connectés. S’ils ne sont pas connectés, nous les redirigerons vers la page de connexion afin qu’ils puissent se connecter.

La mise en œuvre de cette logique est assez facile. Tout ce que nous devons faire est d’ajouter un attribut de filtre [Autoriser] à nos méthodes d’action Créer comme ça :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC prend en charge la capacité de créer des « filtres d’action » qui peuvent être utilisés pour implémenter une logique ré-utilisable qui peut être appliquée de façon déclarative aux méthodes d’action. Le filtre [Authorize] est l’un des filtres d’action intégrés fournis par ASP.NET MVC, et il permet à un développeur d’appliquer de façon déclarative les règles d’autorisation aux méthodes d’action et aux classes de contrôleur.

Lorsqu’il est appliqué sans aucun paramètre (comme ci-dessus), le filtre [Autoriser] impose que l’utilisateur qui effectue la demande de méthode d’action doit être connecté et qu’il redirigera automatiquement le navigateur vers l’URL de connexion s’il ne le fait pas. Lorsque vous effectuez cette redirection, l’URL initialement demandée est passée comme argument de requête (par exemple: /Compte/LogOn? RetourUrl%%2fDinners%2fCreate). Le AccountController redirigera ensuite l’utilisateur vers l’URL initialement demandée une fois qu’il se connectera.

Le filtre [Autoriser] prend en charge la possibilité de spécifier une propriété « Utilisateurs » ou « Rôles » qui peut être utilisée pour exiger que l’utilisateur soit à la fois connecté et dans une liste d’utilisateurs autorisés ou d’un membre d’un rôle de sécurité autorisé. Par exemple, le code ci-dessous permet uniquement à deux utilisateurs spécifiques, "scottgu" et "billg", d’accéder à l’URL /Dîners/Créer:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

L’intégration de noms d’utilisateur spécifiques dans le code a tendance à être assez in maintainable cependant. Une meilleure approche consiste à définir les « rôles » de niveau supérieur que le code vérifie, puis à cartographier les utilisateurs dans le rôle à l’aide d’une base de données ou d’un système d’annuaire actif (permettant à la liste de cartographie utilisateur réelle d’être stockée à l’extérieur à partir du code). ASP.NET comprend une API intégrée de gestion des rôles ainsi qu’un ensemble intégré de fournisseurs de rôles (y compris ceux pour SQL et Active Directory) qui peuvent aider à effectuer cette cartographie utilisateur/rôle. Nous pourrions alors mettre à jour le code uniquement pour permettre aux utilisateurs dans un rôle spécifique "admin" d’accéder à l’URL /Dîners/Créer:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Utilisation de la propriété User.Identity.Name lors de la création de dîners

Nous pouvons récupérer le nom d’utilisateur de l’utilisateur actuellement connecté d’une demande en utilisant la propriété User.Identity.Name exposée sur la classe de base Controller.

Plus tôt, lorsque nous avons mis en œuvre la version HTTP-POST de notre méthode d’action Create() nous avions codé dur la propriété "HostedBy" du dîner à une chaîne statique. Nous pouvons maintenant mettre à jour ce code à la place d’utiliser la propriété User.Identity.Name, ainsi que d’ajouter automatiquement un RSVP pour l’hôte de créer le dîner:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Parce que nous avons ajouté un attribut [Autoriser] à la méthode Créer() ASP.NET MVC s’assure que la méthode d’action ne s’exécute que si l’utilisateur visitant l’URL /Dîners/Créer est connecté sur le site. En tant que tel, la valeur de la propriété User.Identity.Name contiendra toujours un nom d’utilisateur valide.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Utilisation de la propriété User.Identity.Name lors de l’édition des dîners

Ajoutons maintenant une certaine logique d’autorisation qui restreint les utilisateurs afin qu’ils ne puissent modifier que les propriétés des dîners qu’ils hébergent eux-mêmes.

Pour aider à cela, nous allons d’abord ajouter un "IsHostedBy (nom d’utilisateur)" méthode d’aide à notre objet Dîner (dans la classe partielle Dinner.cs que nous avons construit plus tôt). Cette méthode d’aide retourne vraie ou fausse selon qu’un nom d’utilisateur fourni correspond à la propriété Dinner HostedBy, et encapsule la logique nécessaire pour effectuer une comparaison de chaîne insensible cas d’entre eux:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Nous ajouterons ensuite un attribut [Autoriser] aux méthodes d’action Edit () au sein de notre classe DinnersController. Cela permettra de s’assurer que les utilisateurs doivent être connectés pour demander une URL */Dîners/Edit/[id].*

Nous pouvons ensuite ajouter du code à nos méthodes d’édition qui utilise la méthode d’aide Dinner.IsHostedBy (nom d’utilisateur) pour vérifier que l’utilisateur connecté correspond à l’hôte du dîner. Si l’utilisateur n’est pas l’hôte, nous afficherons une vue "InvalidOwner" et met fin à la demande. Le code pour ce faire ressemble ci-dessous:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Nous pouvons ensuite cliquer à droite sur le répertoire «Vues-Dîners» et choisir la commande de menu Add-View&gt;pour créer une nouvelle vue "InvalidOwner". Nous allons le remplir avec le message d’erreur ci-dessous:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Et maintenant, quand un utilisateur tente de modifier un dîner qu’il ne possède pas, il recevra un message d’erreur :

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Nous pouvons répéter les mêmes étapes pour les méthodes d’action Supprimer () au sein de notre contrôleur pour verrouiller l’autorisation de supprimer les dîners ainsi, et de s’assurer que seul l’hôte d’un dîner peut le supprimer.

### <a name="showinghiding-edit-and-delete-links"></a>Afficher/Cacher modifier et supprimer les liens

Nous sommes en lien avec la méthode d’action Edit and Delete de notre classe DinnersController à partir de notre URL Détails:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Actuellement, nous montrons les liens d’action Edit and Delete indépendamment du fait que le visiteur à l’URL des détails est l’hôte du dîner. Changeons cela de sorte que les liens ne sont affichés que si l’utilisateur invité est le propriétaire du dîner.

La méthode d’action Détails () dans notre DinnersController récupère un objet de dîner et le transmet ensuite comme objet modèle à notre modèle de vue :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Nous pouvons mettre à jour notre modèle de vue pour afficher sous condition / cacher les liens Modifier et Supprimer en utilisant la méthode d’aide Dinner.IsHostedBy() comme ci-dessous:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Étapes suivantes

Examinons maintenant comment nous pouvons permettre aux utilisateurs authentifiés de RSVP pour les dîners à l’aide d’AJAX.

> [!div class="step-by-step"]
> [Suivant précédent](implement-efficient-data-paging.md)
> [Next](use-ajax-to-deliver-dynamic-updates.md)
