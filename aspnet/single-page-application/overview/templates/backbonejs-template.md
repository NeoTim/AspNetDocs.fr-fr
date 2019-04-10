---
uid: single-page-application/overview/templates/backbonejs-template
title: Modèle backbone | Microsoft Docs
author: madskristensen
description: Modèle SPA backbone.js
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 8148974eacd1db05947ba54fe40776df69f92290
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59404115"
---
# <a name="backbone-template"></a>Modèle Backbone

par [Mads Kristensen](https://github.com/madskristensen)

> Le modèle de SPA principal a été écrit par Kazi Manzur Rashid
> 
> [Télécharger le modèle SPA Backbone.js](https://go.microsoft.com/fwlink/?LinkId=293631)


Le modèle Backbone.js SPA est conçu pour vous aider à créer rapidement des applications web côté client interactives à l’aide [Backbone.js.](http://backbonejs.org/)

Le modèle fournit un squelette initial pour le développement d’une application Backbone.js dans ASP.NET MVC. Prêt à l’emploi, il fournit les fonctionnalités de connexion utilisateur de base, y compris la réinitialisation de mot de passe d’inscription, la connexion, l’utilisateur et la confirmation de l’utilisateur avec des modèles de messagerie de base.

Configuration requise :

- [Mise à jour ASP.NET et Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Créer un projet de modèle de colonne vertébrale

Téléchargez et installez le modèle en cliquant sur le bouton Télécharger ci-dessus. Le modèle est empaqueté comme un fichier d’Extension Visual Studio (VSIX). Vous devrez peut-être redémarrer Visual Studio.

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud. Sous **Visual C#**, sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**. Nommez le projet et cliquez sur **OK**.

![](backbonejs-template/_static/image1.png)

Dans le **nouveau projet** Assistant, sélectionnez projet de SPA Backbone.js.

![](backbonejs-template/_static/image2.png)

Appuyez sur Ctrl + F5 pour générer et exécuter l’application sans débogage, ou appuyez sur F5 pour exécuter avec débogage.

![](backbonejs-template/_static/image3.png)

Cliquer sur « Mon compte » pour afficher la page de connexion :

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Procédure pas à pas : Code client

Nous allons démarre avec le côté client. Les scripts d’application client se trouvent dans le dossier ~/Scripts/application. L’application est écrite [TypeScript](http://www.typescriptlang.org/) (fichiers .ts) qui sont compilé dans JavaScript (fichiers .js).

**Application**

`Application` est défini dans application.ts. Cet objet initialise l’application et agit comme l’espace de noms racine. Il gère les informations de configuration et l’état qui sont partagées entre l’application, telles que si l’utilisateur est connecté.

Le `application.start` méthode crée les affichages modaux et attache les gestionnaires d’événements pour les événements de niveau application, telles que l’authentification de l’utilisateur. Ensuite, il crée le routeur par défaut et vérifie si n’importe quelle URL côté client est spécifiée. Si non, il redirige vers l’url par défaut (#! /).

**Événements**

Les événements sont toujours importantes lors de développement faiblement couplés de composants. Applications effectuent souvent plusieurs opérations en réponse à une action de l’utilisateur. Réseau principal fournit des événements intégrés avec les composants, tels que la Collection et modèle vue. Au lieu de créer des interdépendances entre ces composants, le modèle utilise un modèle de « pub/sub » : Le `events` objet, défini dans events.ts, agit comme un concentrateur d’événements pour la publication et abonnement aux événements de l’application. Le `events` objet est un singleton. Le code suivant montre comment s’abonner à un événement, puis déclencher l’événement :

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Routeur**

Dans Backbone.js, un routeur fournit des méthodes de routage de pages du côté client et en les connectant aux événements et actions. Le modèle définit un seul routeur dans router.ts. Le routeur crée les vues activables et conserve l’état en changeant d’affichage. (Vues activables sont décrits dans la section suivante.) Initialement, le projet comporte deux vues factices, propos et à domicile. Il a également une vue NotFound, qui s’affiche si l’itinéraire n’est pas connu.

**Affichages**

Les vues sont définies dans ~/Scripts/application/vues. Il existe deux types de vues, les vues activables et les vues de la boîte de dialogue modale. Vues activables sont appelés par le routeur. Lorsqu’une vue activables est affichée, toutes les autres vues activables deviennent inactifs. Pour créer une vue activables, étendre l’affichage avec le `Activable` objet :

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Extension avec `Activable` ajoute deux nouvelles méthodes à la vue, `activate` et `deactivate`. Le routeur appelle ces méthodes pour activer et désactiver la vue.

Affichages modaux sont implémentés en tant que [Twitter Bootstrap](http://twitter.github.com/bootstrap/) boîtes de dialogue modales. Le `Membership` et `Profile` affichages sont modales. Vues de modèle peuvent être appelées par les événements d’application. Par exemple, dans le `Navigation` , en cliquant sur le lien « Mon compte » affiche soit la `Membership` vue ou la `Profile` vue, selon que l’utilisateur est connecté. Le `Navigation` attache les gestionnaires d’événements pour les éléments enfants qui ont click le `data-command` attribut. Voici le balisage HTML :

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Voici le code dans navigation.ts pour raccorder les événements :

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modèles**

Les modèles sont définis dans ~/Scripts/application/modèles. Les modèles ont tous trois opérations de base : attributs par défaut, les règles de validation et un point de terminaison côté serveur. Voici un exemple typique :

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Plug-ins**

Le dossier ~/Scripts/application/lib contient plusieurs plug-ins de jQuery pratique. Le fichier form.ts définit un plug-in pour l’utilisation des données de formulaire. Souvent, vous devez sérialiser ou désérialiser des données de formulaire et afficher des erreurs de validation de modèle. Le plug-in de form.ts possède des méthodes telles que `serializeFields`, `deserializeFields`, et `showFieldErrors`. L’exemple suivant montre comment sérialiser un formulaire à un modèle.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Le plug-in de flashbar.ts offre différents types de messages de commentaires à l’utilisateur. Les méthodes sont `$.showSuccessbar`, `$.showErrorbar` et `$.showInfobar`. Dans les coulisses, il utilise les alertes Twitter Bootstrap pour afficher les messages bien animées.

Le plug-in de confirm.ts remplace le navigateur confirmer la boîte de dialogue, bien que l’API est quelque peu différent :

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Procédure pas à pas : Code serveur

Maintenant nous allons examiner le côté serveur.

**Contrôleurs**

Dans une application à page unique, le serveur ne joue qu’un rôle limité dans l’interface utilisateur. En règle générale, le serveur restitue la page initiale envoie et reçoit des données JSON.

Le modèle a deux contrôleurs MVC : `HomeController` restitue la page initiale, et `SupportsController` sert à confirmer les nouveaux comptes d’utilisateur et de réinitialiser les mots de passe. Tous les autres contrôleurs dans le modèle sont des contrôleurs d’API Web ASP.NET, qui envoient et reçoivent des données JSON. Par défaut, les contrôleurs utilisent le nouveau `WebSecurity` classe pour effectuer des tâches relatives à l’utilisateur. Toutefois, ils ont également des constructeurs facultatifs qui vous permettent de passer dans les délégués pour effectuer ces tâches. Cela facilite les tests et vous permet de remplacer `WebSecurity` avec quelque chose d’autre, à l’aide d’un conteneur IoC. Voici un exemple :

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Affichages

Les vues sont conçues pour être modulaire : Chaque section d’une page possède sa propre vue dédié. Dans une application à page unique, il est courant d’inclure des vues qui n’ont pas de n’importe quel contrôleur correspondant. Vous pouvez inclure une vue en appelant `@Html.Partial('myView')`, mais ceci devient fastidieux. Pour simplifier ce processus, le modèle définit une méthode d’assistance, `IncludeClientViews`, qui affiche toutes les vues dans un dossier spécifié :

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Si le nom du dossier n’est pas spécifié, le nom de dossier par défaut est « ClientViews ». Si votre vue client utilise également des vues partielles, nommez la vue partielle avec un caractère de soulignement (par exemple, `_SignUp`). Le `IncludeClientViews` méthode exclut toutes les vues dont le nom commence par un trait de soulignement. Pour inclure une vue partielle dans la vue du client, appelez `Html.ClientView('SignUp')` au lieu de `Html.Partial('_SignUp')`.

**Envoi de courriers électroniques**

Pour envoyer un e-mail, le modèle utilise [Postal](http://aboutcode.net/postal). Toutefois, il est abstrait Postal du reste du code avec le `IMailer` interface, donc vous pouvez facilement le remplacer par une autre implémentation. Les modèles d’e-mail sont situés dans le dossier de vues et des messages électroniques. Adresse de messagerie de l’expéditeur est spécifié dans le fichier web.config, dans le `sender.email` clé de la **appSettings** section. Également, lorsque `debug="true"` dans le fichier web.config, l’application ne nécessite pas d’e-mail de confirmation utilisateur pour accélérer le développement.

## <a name="github"></a>GitHub

Vous trouverez également le modèle Backbone.js SPA sur [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
