---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Itération #7 - Ajouter la fonctionnalité Ajax (VB) Microsoft Docs'
author: rick-anderson
description: Dans la septième itération, nous améliorons la réactivité et la performance de notre application en ajoutant un soutien pour Ajax.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: 04eaaa129a56b765c090e64118ed528c65987b50
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542298"
---
# <a name="iteration-7--add-ajax-functionality-vb"></a>Itération #7 : Ajouter des fonctionnalités Ajax (VB)

par [Microsoft](https://github.com/microsoft)

[Code de téléchargement](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> Dans la septième itération, nous améliorons la réactivité et la performance de notre application en ajoutant un soutien pour Ajax.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Construire une application de gestion des contacts ASP.NET MVC (VB)

Dans cette série de tutoriels, nous construisons toute une application de gestion de contact du début à la fin. L’application Contact Manager vous permet de stocker les coordonnées - noms, numéros de téléphone et adresses e-mail - pour une liste de personnes.

Nous construisons l’application sur plusieurs itérations. À chaque itération, nous améliorons progressivement l’application. Le but de cette approche d’itération multiple est de vous permettre de comprendre la raison de chaque changement.

- Itération #1 - Créer l’application. Dans la première itération, nous créons le Gestionnaire de Contact de la manière la plus simple possible. Nous ajoutons un support pour les opérations de base de base de base : Créer, lire, mettre à jour et supprimer (CRUD).

- Itération #2 - Rendre l’application belle. Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page principale de vue par défaut ASP.NET MVC et la feuille de style en cascade.

- Itération #3 - Ajouter la validation du formulaire. Dans la troisième itération, nous ajoutons la validation de forme de base. Nous empêchons les gens de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses e-mail et les numéros de téléphone.

- Itération #4 - Rendre l’application lâchement couplée. Dans cette quatrième itération, nous profitons de plusieurs modèles de conception logicielle pour faciliter le maintien et la modification de l’application Contact Manager. Par exemple, nous refactorrons notre application pour utiliser le modèle de dépôt et le modèle d’injection de dépendance.

- Itération #5 - Créer des tests unitaires. Dans la cinquième itération, nous rendons notre application plus facile à maintenir et à modifier en ajoutant des tests unitaires. Nous nous moquons de nos classes de modèles de données et construisons des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6 - Utilisez le développement axé sur les tests. Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code contre les tests unitaires. Dans cette itération, nous ajoutons des groupes de contact.

- Itération #7 - Ajouter la fonctionnalité Ajax. Dans la septième itération, nous améliorons la réactivité et la performance de notre application en ajoutant un soutien pour Ajax.

## <a name="this-iteration"></a>Cette Itération

Dans cette itération de l’application Contact Manager, nous refactorrons notre application pour faire usage d’Ajax. En profitant de l’Ajax, nous rendons notre application plus réactive. Nous pouvons éviter de rendre une page entière lorsque nous avons besoin de mettre à jour seulement une certaine région dans une page.

Nous refactorrons notre vue Index afin de ne pas avoir besoin de redisjouer la page entière chaque fois que quelqu’un choisit un nouveau groupe de contact. Au lieu de cela, lorsque quelqu’un clique sur un groupe de contact, nous allons simplement mettre à jour la liste des contacts et laisser le reste de la page seul.

Nous modifierons également la façon dont notre lien de suppression fonctionne. Au lieu d’afficher une page de confirmation séparée, nous afficherons un dialogue de confirmation JavaScript. Si vous confirmez que vous souhaitez supprimer un contact, une opération HTTP DELETE est effectuée contre le serveur pour supprimer l’enregistrement de contact de la base de données.

En outre, nous profiterons de jQuery pour ajouter des effets d’animation à notre vue Index. Nous afficherons une animation lorsque la nouvelle liste de contacts sera récupérée sur le serveur.

Enfin, nous tirerons parti de la ASP.NET support-cadre AJAX pour gérer l’historique du navigateur. Nous créerons des points d’histoire chaque fois que nous effectuerons un appel Ajax pour mettre à jour la liste de contacts. De cette façon, le navigateur vers l’arrière et vers l’avant des boutons fonctionnera.

## <a name="why-use-ajax"></a>Pourquoi utiliser Ajax?

L’utilisation de l’Ajax a de nombreux avantages. Tout d’abord, l’ajout de fonctionnalités Ajax à une application se traduit par une meilleure expérience utilisateur. Dans une application Web normale, la page entière doit être postée sur le serveur chaque fois qu’un utilisateur effectue une action. Chaque fois que vous effectuez une action, le navigateur se verrouille et l’utilisateur doit attendre jusqu’à ce que la page entière est récupérée et redisjouée.

Ce serait une expérience inacceptable dans le cas d’une application de bureau. Mais, traditionnellement, nous vivions avec cette mauvaise expérience utilisateur dans le cas d’une application web parce que nous ne savions pas que nous pouvions faire mieux. Nous avons pensé que c’était une limitation des applications web alors que, en réalité, c’était juste une limitation de notre imagination.

Dans une application Ajax, vous n’avez pas besoin de mettre un terme à l’expérience utilisateur juste pour mettre à jour une page. Au lieu de cela, vous pouvez effectuer une demande asynchrone en arrière-plan pour mettre à jour la page. Vous ne forcez pas l’utilisateur à attendre pendant qu’une partie de la page est mise à jour.

En profitant de l’Ajax, vous pouvez également améliorer les performances de votre application. Considérez comment fonctionne l’application Contact Manager dès maintenant sans fonctionnalité Ajax. Lorsque vous cliquez sur un groupe de contact, la vue de l’index entier doit être redisjouée. La liste des contacts et la liste des groupes de contact doivent être récupérées à partir du serveur de base de données. Toutes ces données doivent être transmises à travers le fil du serveur Web au navigateur Web.

Après avoir ajouté la fonctionnalité Ajax à notre application, cependant, nous pouvons éviter de redisplaying la page entière quand un utilisateur clique sur un groupe de contact. Nous n’avons plus besoin de saisir les groupes de contact de la base de données. Nous n’avons pas non plus besoin de pousser la vue de l’index entier à travers le fil. En profitant d’Ajax, nous réduisons la quantité de travail que notre serveur de base de données doit effectuer et nous réduisons la quantité de trafic réseau requis par notre application.

## <a name="don-t-be-afraid-of-ajax"></a>N’ayez pas peur de l’Ajax

Certains développeurs évitent d’utiliser Ajax parce qu’ils s’inquiètent des navigateurs à un niveau bas. Ils veulent s’assurer que leurs applications Web fonctionneront toujours lorsqu’ils sont accessibles par un navigateur qui ne prend pas en charge JavaScript. Parce que l’Ajax dépend de JavaScript, certains développeurs évitent d’utiliser Ajax.

Cependant, si vous faites attention sur la façon dont vous implémentez Ajax alors vous pouvez construire des applications qui fonctionnent avec les navigateurs haut de niveau et à la baisse. Notre application Contact Manager fonctionnera avec les navigateurs qui prennent en charge JavaScript et les navigateurs qui ne le font pas.

Si vous utilisez l’application Contact Manager avec un navigateur qui prend en charge JavaScript, vous aurez une meilleure expérience utilisateur. Par exemple, lorsque vous cliquez sur un groupe de contact, seule la région de la page qui affiche les contacts sera mise à jour.

Si, d’autre part, vous utilisez l’application Contact Manager avec un navigateur qui ne prend pas en charge JavaScript (ou qui a JavaScript désactivé), alors vous aurez une expérience utilisateur un peu moins souhaitable. Par exemple, lorsque vous cliquez sur un groupe de contact, l’ensemble de la vue Index doit être affiché sur le navigateur afin d’afficher la liste correspondante des contacts.

## <a name="adding-the-required-javascript-files"></a>Ajout des fichiers JavaScript requis

Nous devrons utiliser trois fichiers JavaScript pour ajouter la fonctionnalité Ajax à notre application. Ces trois fichiers sont inclus dans le dossier Scripts d’une nouvelle application ASP.NET MVC.

Si vous prévoyez d utiliser Ajax dans plusieurs pages de votre application, il est logique d inclure les fichiers JavaScript requis dans la page principale de votre application. De cette façon, les fichiers JavaScript seront inclus automatiquement dans toutes les pages de votre application.

Ajouter le JavaScript suivant &lt;&gt; comprend à l’intérieur de l’étiquette de tête de votre page principale de vue:

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactoring the Index View to use Ajax

Commençons par modifier notre vue d’index de sorte que le clic d’un groupe de contact ne met à jour que la région de la vue qui affiche les contacts. La boîte rouge de la figure 1 contient la région que nous voulons mettre à jour.

[![Mise à jour uniquement des contacts](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Figure 01**: Mise à jour uniquement des contacts ([Cliquez pour voir l’image grandeur nature](iteration-7-add-ajax-functionality-vb/_static/image2.png))

La première étape consiste à séparer la partie de l’opinion que nous voulons mettre à jour asynchroneously dans une partielle séparée (voir le contrôle de l’utilisateur). La section de l’indice de vue qui affiche le tableau des contacts a été déplacée dans la partie dans la liste 1.

**Liste 1 - Vues-ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Notez que la partie dans la liste 1 a un modèle différent de celui de l’indice. L’attribut *d’Inherits* &lt;dans&gt; la directive % de page en % précise&lt;que&gt; l’attribut partiel hérite de la classe ViewUserControl Group.

La vue de l’indice mise à jour est contenue dans la liste 2.

**Liste 2 - Views-Contact.Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Il ya deux choses que vous devriez remarquer sur la vue mise à jour dans la liste 2. Tout d’abord, notez que tout le contenu déplacé dans la partie est remplacé par un appel à Html.RenderPartial (). La méthode Html.RenderPartial() est appelée lorsque la vue d’index est demandée pour la première fois afin d’afficher l’ensemble initial de contacts.

Deuxièmement, notez que le Html.ActionLink() utilisé pour afficher les groupes de contact a été remplacé par un Ajax.ActionLink(). L’Ajax.ActionLink () est appelé avec les paramètres suivants:

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

Le premier paramètre représente le texte à afficher pour le lien, le deuxième paramètre représente les valeurs de l’itinéraire, et le troisième paramètre représente les options Ajax. Dans ce cas, nous utilisons l’option UpdateTargetId&gt; Ajax pour pointer vers l’étiquette de div HTML &lt;que nous voulons mettre à jour après la demande Ajax terminée. Nous voulons mettre &lt;à&gt; jour l’étiquette de div avec la nouvelle liste de contacts.

La méthode index() mise à jour du contrôleur de contact est contenue dans la liste 3.

**Liste 3 - Controllers-ContactController.vb (méthode de l’indice)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

L’action index mise à jour() renvoie conditionnellement l’une des deux choses. Si l’action Index() est invoquée par une demande d’Ajax, le contrôleur renvoie une partie. Dans le cas contraire, l’action Index() renvoie une vue entière.

Notez que l’action Index() n’a pas besoin de retourner autant de données lorsqu’elle est invoquée par une demande Ajax. Dans le cadre d’une demande normale, l’action Index renvoie une liste de tous les groupes de contact et du groupe de contact sélectionné. Dans le cadre d’une demande Ajax, l’action Index() ne renvoie que le groupe sélectionné. Ajax signifie moins de travail sur votre serveur de base de données.

Notre vue Index modifiée fonctionne dans le cas des navigateurs haut de niveau et à la baisse. Si vous cliquez sur un groupe de contact, et que votre navigateur prend en charge JavaScript, la région de la vue qui contient la liste des contacts est mise à jour. Si, d’autre part, votre navigateur ne prend pas en charge JavaScript, alors la vue entière est mise à jour.

Notre vue Index mise à jour a un problème. Lorsque vous cliquez sur un groupe de contact, le groupe sélectionné n’est pas mis en évidence. Étant donné que la liste des groupes est affichée à l’extérieur de la région qui est mise à jour lors d’une demande Ajax, le bon groupe ne se fait pas mettre en évidence. Nous allons résoudre ce problème dans la section suivante.

## <a name="adding-jquery-animation-effects"></a>Ajout d’effets d’animation jQuery

Normalement, lorsque vous cliquez sur un lien dans une page Web, vous pouvez utiliser la barre de progression du navigateur pour détecter si oui ou non le navigateur est activement chercher le contenu mis à jour. Lors de l’exécution d’une demande Ajax, d’autre part, la barre de progression du navigateur ne montre aucun progrès. Cela peut rendre les utilisateurs nerveux. Comment savez-vous si le navigateur a gelé?

Il existe plusieurs façons d’indiquer à un utilisateur que le travail est effectué lors de l’exécution d’une demande Ajax. Une approche consiste à afficher une animation simple. Par exemple, vous pouvez vous faner une région lorsqu’une demande d’Ajax commence et s’estomper dans la région lorsque la demande sera terminée.

Nous utiliserons la bibliothèque jQuery qui est incluse avec le cadre Microsoft ASP.NET MVC, pour créer les effets d’animation. La vue de l’indice mise à jour est contenue dans la liste 4.

**Liste 4 - Views-Contact.Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Notez que la vue Index mise à jour contient trois nouvelles fonctions JavaScript. Les deux premières fonctions utilisent jQuery pour s’estomper et s’estomper dans la liste des contacts lorsque vous cliquez sur un nouveau groupe de contact. La troisième fonction affiche un message d’erreur lorsqu’une demande d’Ajax entraîne une erreur (par exemple, un délai d’attente en réseau).

La première fonction s’occupe également de mettre en évidence le groupe sélectionné. Un attribut sélectionné par classeMD est ajouté à l’élément parent (l’élément LI) de l’élément cliqué. Encore une fois, jQuery permet de sélectionner facilement le bon élément et d’ajouter la classe CSS.

Ces scripts sont liés aux liens de groupe avec l’aide du paramètre Ajax.ActionLink() AjaxOptions. L’appel de méthode Ajax.ActionLink() mis à jour ressemble à ceci :

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Ajout d’un support d’historique de navigateur

Normalement, lorsque vous cliquez sur un lien pour mettre à jour une page, l’historique de votre navigateur est mis à jour. De cette façon, vous pouvez cliquer sur le bouton De retour du navigateur pour revenir dans le temps à l’état précédent de la page. Par exemple, si vous cliquez sur le groupe de contact Friends, puis cliquez sur le groupe de contact Business, vous pouvez cliquer sur le bouton De retour du navigateur pour revenir à l’état de la page lorsque le groupe de contact Friends a été sélectionné.

Malheureusement, l’exécution d’une demande Ajax ne met pas à jour automatiquement l’historique du navigateur. Si vous cliquez sur un groupe de contact, et la liste des contacts correspondants est récupérée avec une demande Ajax, alors l’historique du navigateur n’est pas mis à jour. Vous ne pouvez pas utiliser le bouton Navigateur Retour pour revenir à un groupe de contact après avoir sélectionné un nouveau groupe de contact.

Si vous voulez que les utilisateurs soient en mesure d’utiliser le bouton De retour du navigateur après avoir effectué des demandes Ajax, alors vous devez effectuer un peu plus de travail. Vous devez profiter de la fonctionnalité de gestion de l’historique du navigateur intégrée dans le cadre ASP.NET AJAX.

ASP.NET’historique du navigateur AJAX, vous devez faire trois choses:

1. Activez l’historique du navigateur en définissant la propriété enableBrowserHistory.
2. Enregistrez les points d’histoire lorsque l’état d’une vue change en appelant la méthode addHistoryPoint().
3. Reconstruire l’état de la vue lorsque l’événement de navigation est soulevé.

La vue de l’indice mise à jour est contenue dans la liste 5.

**Liste 5 - Views-Contact.Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

Dans La liste 5, l’historique du navigateur est activé dans la fonction pageInit() . La fonction pageInit() est également utilisée pour configurer le gestionnaire d’événements pour l’événement de navigation. L’événement de navigation est soulevé chaque fois que le navigateur Forward ou Back bouton provoque l’état de la page à changer.

La méthode beginContactList() s’appelle lorsque vous cliquez sur un groupe de contact. Cette méthode crée un nouveau point d’histoire en appelant la méthode addHistoryPoint(). L’id du groupe de contact cliqué est ajouté à l’historique.

L’id de groupe est récupéré à partir d’un attribut expando sur le lien de groupe de contact. Le lien est rendu avec l’appel suivant à Ajax.ActionLink ().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Le dernier paramètre passé à l’Ajax.ActionLink () ajoute un attribut expando nommé groupidé au lien (lowercase pour la compatibilité XHTML).

Lorsqu’un utilisateur touche le navigateur Sur le bouton Retour ou Avant, l’événement de navigation est déclenché et la méthode de navigation () s’appelle. Cette méthode met à jour les contacts affichés dans la page pour correspondre à l’état de la page qui correspond au point d’historique du navigateur passé à la méthode de navigation.

## <a name="performing-ajax-deletes"></a>Exécution Ajax Supprime

Actuellement, pour supprimer un contact, vous devez cliquer sur le lien Supprimer, puis cliquer sur le bouton Supprimer affiché dans la page de confirmation de suppression (voir la figure 2). Cela semble être beaucoup de demandes de page pour faire quelque chose de simple comme la suppression d’un enregistrement de base de données.

[![La page de confirmation de suppression](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Figure 02**: La page de confirmation de suppression[(Cliquez pour voir l’image grandeur nature](iteration-7-add-ajax-functionality-vb/_static/image4.png))

Il est tentant de sauter la page de confirmation de suppression et de supprimer un contact directement à partir de la vue index. Vous devriez éviter cette tentation parce que la prise de cette approche ouvre votre application à des trous de sécurité. En général, vous ne voulez pas effectuer une opération HTTP GET lors de l’invocation d’une action qui modifie l’état de votre application web. Lors de l’exécution d’une suppression, vous souhaitez effectuer un MESSAGE HTTP, ou mieux encore, une opération HTTP DELETE.

Le lien Supprimer est contenu dans la Liste de contact partielle. Une version mise à jour de la liste de contact partielle est contenue dans la liste 6.

**Liste 6 - Views-Contact-ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Le lien Supprimer est rendu avec l’appel suivant à la méthode Ajax.ImageActionLink() :

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> L’Ajax.ImageActionLink() n’est pas une partie standard du cadre ASP.NET MVC. L’Ajax.ImageActionLink() est une méthode d’aide personnalisée incluse dans le projet Contact Manager.

Le paramètre AjaxOptions a deux propriétés. Tout d’abord, la propriété Confirm est utilisée pour afficher un dialogue de confirmation JavaScript popup. Deuxièmement, la propriété HttpMethod est utilisée pour effectuer une opération HTTP DELETE.

La liste 7 contient une nouvelle action AjaxDelete() qui a été ajoutée au contrôleur de contact.

**Liste 7 - Controllers-ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

L’action AjaxDelete() est décorée d’un attribut AcceptVerbs. Cet attribut empêche l’action d’être invoquée, sauf par toute opération HTTP autre qu’une opération HTTP DELETE. En particulier, vous ne pouvez pas invoquer cette action avec un GET HTTP.

Après avoir supprimé l’enregistrement de base de données, vous devez afficher la liste mise à jour des contacts qui ne contiennent pas l’enregistrement supprimé. La méthode AjaxDelete() renvoie la Liste de contact partielle et la liste mise à jour des contacts.

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté la fonctionnalité Ajax à notre application Contact Manager. Nous avons utilisé Ajax pour améliorer la réactivité et les performances de notre application.

Tout d’abord, nous avons refactorisé la vue Index de sorte que le fait de cliquer sur un groupe de contact ne met pas à jour l’ensemble de la vue. Au lieu de cela, en cliquant sur un groupe de contact ne met à jour la liste des contacts.

Ensuite, nous avons utilisé des effets d’animation jQuery pour s’estomper et s’estomper dans la liste des contacts. L’ajout d’animation à une application Ajax peut être utilisé pour fournir aux utilisateurs de l’application l’équivalent d’une barre de progression du navigateur.

Nous avons également ajouté le support d’historique du navigateur à notre application Ajax. Nous avons permis aux utilisateurs de cliquer sur les boutons Back and Forward du navigateur pour modifier l’état de la vue de l’index.

Enfin, nous avons créé un lien de suppression qui prend en charge les opérations HTTP DELETE. En effectuant les suppressions d’Ajax, nous permettons aux utilisateurs de supprimer les enregistrements de base de données sans exiger de l’utilisateur qu’il demande une page de confirmation de suppression supplémentaire.

> [!div class="step-by-step"]
> [Précédent](iteration-6-use-test-driven-development-vb.md)
