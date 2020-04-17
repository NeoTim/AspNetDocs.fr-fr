---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fournir cruD (Créer, lire, mettre à jour, supprimer) le support d’entrée de formulaire de données (fr) Microsoft Docs
author: rick-anderson
description: Étape 5 montre comment prendre notre classe DinnersController plus loin en permettant de prendre en charge l’édition, la création et la suppression des dîners avec elle ainsi.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 2b75a7eda8bce4baa25d92626639f4d904eb363a
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542623"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fournir une prise en charge des entrées dans les formulaires de données CRUD (créer, lire, mettre à jour, supprimer)

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 5 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> Étape 5 montre comment prendre notre classe DinnersController plus loin en permettant de prendre en charge l’édition, la création et la suppression des dîners avec elle ainsi.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner Étape 5: Créer, mettre à jour, Supprimer les scénarios de formulaire

Nous avons introduit des contrôleurs et des vues, et couvert la façon de les utiliser pour mettre en œuvre une expérience de liste / détails pour les dîners sur place. Notre prochaine étape sera d’aller plus loin dans notre classe DinnersController et de faciliter l’édition, la création et la suppression des dîners avec elle.

### <a name="urls-handled-by-dinnerscontroller"></a>URL manipulées par DinnersController

Nous avons déjà ajouté des méthodes d’action à DinnersController qui a mis en œuvre le soutien pour deux URL: */Dîners* et */Dîners/Détails/[id]*.

| **URL** | **Verbe** | **Objectif** |
| --- | --- | --- |
| */Dîners/* | GET | Affichez une liste HTML des dîners à venir. |
| */Dîners/Détails/[id]* | GET | Afficher les détails d’un dîner spécifique. |

Nous allons maintenant ajouter des méthodes d’action pour mettre en œuvre trois URL supplémentaires: */Dîners/Edit/[id]*, */Dinners/Create*, et */Dinners/Delete/[id]*. Ces URL permettront de prendre en charge l’édition des dîners existants, la création de nouveaux dîners et la suppression des dîners.

Nous soutiendrons à la fois les interactions AVEC les verbes HTTP GET et HTTP POST avec ces nouvelles URL. LES demandes DE HTTP GET à ces URL afficheront la vue HTML initiale des données (un formulaire peuplé des données du dîner dans le cas de « modification », un formulaire vierge dans le cas de « créer », et un écran de confirmation supprimer dans le cas de « supprimer »). Les demandes DE HTTP à ces URL enregistreront/mise à jour/supprimeront les données du dîner dans notre DinnerRepository (et de là à la base de données).

| **URL** | **Verbe** | **Objectif** |
| --- | --- | --- |
| */Dîners/Edit/[id]* | GET | Affichez un formulaire HTML modifiable peuplé de données Dîner. |
| POST | Enregistrez les modifications de formulaire pour un dîner particulier dans la base de données. |
| */Dîners/Créer* | GET | Affichez un formulaire HTML vide qui permet aux utilisateurs de définir de nouveaux dîners. |
| POST | Créez un nouveau dîner et enregistrez-le dans la base de données. |
| */Dîners/Supprimer/[id]* | GET | Afficher supprimer l’écran de confirmation. |
| POST | Supprime le dîner spécifié de la base de données. |

### <a name="edit-support"></a>Modifier le support

Commençons par implémenter le scénario "edit".

#### <a name="the-http-get-edit-action-method"></a>La méthode d’action HTTP-GET Edit

Nous allons commencer par mettre en œuvre le comportement HTTP "GET" de notre méthode d’action d’édition. Cette méthode sera invoquée lorsque l’URL */Dîners/Edit/[id]* sera demandée. Notre mise en œuvre ressemblera à :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Le code ci-dessus utilise le DinnerRepository pour récupérer un objet Dinner. Il rend ensuite un modèle de vue à l’aide de l’objet Dinner. Parce que nous n’avons pas explicitement passé un nom de modèle à la *méthode d’aide View(),* il utilisera le chemin par défaut basé sur la convention pour résoudre le modèle de vue: /Views/Dinners/Edit.aspx.

Créons maintenant ce modèle de vue. Pour ce faire, nous cliquerons directement dans la méthode Edit et en sélectionnant la commande de menu contextuelle « Add View » :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Dans le dialogue "Add View", nous indiquerons que nous passons un objet de dîner à notre modèle de vue comme modèle, et choisirons d’auto-échafaudage un modèle "Modifier":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Lorsque nous cliquez sur le bouton "Ajouter", Visual Studio ajoutera un nouveau fichier de modèle de vue "Edit.aspx" pour nous dans l’annuaire "Views-Dîners". Il ouvrira également le nouveau modèle de vue "Edit.aspx" dans l’éditeur de code - peuplé d’une première implémentation d’échafaudages "Edit" comme ci-dessous:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Modifions quelques modifications à l’échafaudage par défaut « éditez » généré, et mettons à jour le modèle de vue d’édition pour avoir le contenu ci-dessous (qui supprime quelques-unes des propriétés que nous ne voulons pas exposer) :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Lorsque nous passons l’application et demandons l’URL *"/Dîners/Edit/1",* nous verrons la page suivante :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

La balisage HTML générée par notre vue ressemble ci-dessous. Il s’agit &lt;d’un HTML standard - avec un élément de formulaire&gt; qui effectue un MESSAGE HTTP à l’URL */Dîners/Edit/1* lorsque le type d’entrée "Enregistrer" &lt;"soumettre" /&gt; bouton est poussé. Un &lt;type d’entrée HTML&gt; "texte" / élément a été la sortie pour chaque propriété modifiable:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() et Html.TextBox() Méthodes d’aide html

Notre modèle de vue "Edit.aspx" utilise plusieurs méthodes "Html Helper": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox(), et Html.ValidationMessage (). En plus de générer une balisage HTML pour nous, ces méthodes d’aide fournissent un support intégré de traitement des erreurs et de validation.

##### <a name="htmlbeginform-helper-method"></a>Méthode d’aide Html.BeginForm()

La méthode d’aide Html.BeginForm() &lt;est&gt; ce que la sortie de l’élément de forme HTML dans notre balisage. Dans notre modèle de vue Edit.aspx, vous remarquerez que nous appliquons une déclaration C "utilisation" lors de l’utilisation de cette méthode. L’accolade bouclée ouverte indique &lt;&gt; le début du contenu du formulaire, et &lt;l’accolade bouclée de fermeture est ce qui indique la fin de l’élément /forme:&gt;

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Alternativement, si vous trouvez l’approche de déclaration «utilisation» contre nature pour un scénario comme celui-ci, vous pouvez utiliser une combinaison Html.BeginForm() et Html.EndForm() (qui fait la même chose):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Appeler Html.BeginForm() sans aucun paramètre lui fera sortir un élément de formulaire qui fait un HTTP-POST à l’URL de la demande actuelle. C’est pourquoi notre vue Edit génère un * &lt;formulaire action"/Dîners/Edit/1" méthode&gt; "post"élément.* Nous aurions pu passer alternativement des paramètres explicites à Html.BeginForm() si nous voulions poster à une URL différente.

##### <a name="htmltextbox-helper-method"></a>Méthode d’aide Html.TextBox()

Notre vue Edit.aspx utilise la méthode d’assistance &lt;Html.TextBox()&gt; pour produire le type d’entrée"/ éléments:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

La méthode Html.TextBox() ci-dessus prend un seul paramètre - qui est &lt;utilisé pour spécifier à la fois les attributs id / nom du type d’entrée "texte" /&gt; élément à la sortie, ainsi que la propriété du modèle pour remplir la valeur de la boîte de texte à partir. Par exemple, l’objet dîner que nous avons passé à la vue Edit avait une valeur de propriété "Titre" de ".NET Futures", et donc notre Html.TextBox ("Title") mode de traitement de la méthode : * &lt;entrée id "Title" name""Title" type "texte" valeur "NET Futures" /&gt;*.

Alternativement, nous pouvons utiliser le premier paramètre Html.TextBox() pour spécifier l’id/nom de l’élément, puis passer explicitement dans la valeur à utiliser comme un deuxième paramètre:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Souvent, nous voulons effectuer le formatage personnalisé sur la valeur qui est la sortie. La méthode statique String.Format() intégrée en .NET est utile pour ces scénarios. Notre modèle de vue Edit.aspx l’utilise pour formater la valeur EventDate (qui est de type DateTime) afin qu’il ne s’affiche pas de secondes pour le temps :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Un troisième paramètre à Html.TextBox() peut être utilisé en option pour produire des attributs HTML supplémentaires. L’extrait de code ci-dessous montre comment rendre un attribut de taille supplémentaire de 30 et &lt;un attribut de classe&gt; «mycssclass» sur le type d’entrée "texte" / élément. Notez comment nous échappons au nom@" character because "de l’attribut de la classe à l’aide d’une « classe » est un mot clé réservé dans C :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Mise en œuvre de la méthode d’action HTTP-POST Edit

Nous avons maintenant la version HTTP-GET de notre méthode d’action Edit mis en œuvre. Lorsqu’un utilisateur demande l’URL */Dîners/Edit/1,* il reçoit une page HTML comme suit :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Appuyer sur le bouton "Enregistrer" provoque un message de formulaire à l’URL &lt;&gt; */Dîners/Edit/1,* et soumet les valeurs du formulaire d’entrée HTML à l’aide du verbe HTTP POST. Mettons maintenant en œuvre le comportement HTTP POST de notre méthode d’action d’édition, qui permettra de sauver le dîner.

Nous allons commencer par ajouter une méthode d’action surchargée "Edit" à notre DinnersController qui a un attribut "AcceptVerbs" sur elle qui indique qu’il gère les scénarios HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Lorsque l’attribut [AcceptVerbs] est appliqué à des méthodes d’action surchargées, ASP.NET MVC traite automatiquement les demandes d’envoi à la méthode d’action appropriée en fonction du verbe HTTP entrant. HTTP POST demandes à */Dinners/Edit/[id]* URL iront à la méthode d’édition ci-dessus, tandis que toutes les autres demandes de verbe HTTP à `[AcceptVerbs]` */Dîners/Edit/[id]* URL iront à la première méthode d’édition que nous avons mis en œuvre (qui n’avait pas d’attribut).

| **Sujet secondaire: Pourquoi différencier via les verbes HTTP?** |
| --- |
| Vous pourriez vous demander pourquoi utilisons-nous une seule URL et différenciation de son comportement via le verbe HTTP? Pourquoi ne pas simplement avoir deux URL distinctes pour gérer les modifications de chargement et d’enregistrement? Par exemple : /Dîners/Modifier/[id] pour afficher le formulaire initial et /Dîners/Enregistrer/[id] pour gérer le formulaire pour l’enregistrer ? L’inconvénient de la publication de deux URL distinctes est que dans les cas où nous publions à /Dîners / Enregistrer/2, puis besoin de redisplay le formulaire HTML en raison d’une erreur d’entrée, l’utilisateur final finira par avoir l’URL /Dîners / Enregistrer / 2 dans la barre d’adresse de leur navigateur (puisque c’était l’URL le formulaire affiché à). Si l’utilisateur final signe ce signet redisplayed page à leur liste de favoris de navigateur, ou copie /coller l’URL et l’envoie par e-mail à un ami, ils finiront par enregistrer une URL qui ne fonctionnera pas à l’avenir (puisque cette URL dépend des valeurs de publication). En exposant une seule URL (comme : /Dîners/Edit/[id]) et en différenciant le traitement de celle-ci par verbe HTTP, il est sécuritaire pour les utilisateurs finaux de marquer la page d’édition et/ou d’envoyer l’URL à d’autres. |

#### <a name="retrieving-form-post-values"></a>Récupération des valeurs post-courriers

Il existe une variété de façons d’accéder aux paramètres de formulaire affichés dans notre méthode HTTP POST "Edit". Une approche simple consiste à simplement utiliser la propriété Demande sur la classe de base Controller pour accéder à la collecte du formulaire et récupérer directement les valeurs affichées :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

L’approche ci-dessus est un peu verbeux, cependant, surtout une fois que nous ajoutons la logique de manipulation d’erreur.

Une meilleure approche pour ce scénario est de tirer parti de la méthode intégrée *UpdateModel()* aide sur la classe de base Controller. Il prend en charge la mise à jour des propriétés d’un objet que nous passons en utilisant les paramètres de formulaire entrants. Il utilise la réflexion pour déterminer les noms de propriété sur l’objet, puis convertit et leur attribue automatiquement des valeurs en fonction des valeurs d’entrée soumises par le client.

Nous pourrions utiliser la méthode UpdateModel() pour simplifier notre action HTTP-POST Edit à l’aide de ce code :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Nous pouvons maintenant visiter l’URL */Dîners/Edit/1,* et changer le titre de notre dîner :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Lorsque nous cliquez sur le bouton "Enregistrer", nous effectuerons un formulaire à notre action Edit, et les valeurs mises à jour seront persistantes dans la base de données. Nous serons ensuite redirigés vers l’URL Détails pour le dîner (qui affichera les valeurs nouvellement enregistrées):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Manipulation des erreurs d’édition

Notre implémentation HTTP-POST actuelle fonctionne très bien, sauf lorsqu’il y a des erreurs.

Lorsqu’un utilisateur fait une erreur d’édition d’un formulaire, nous devons nous assurer que le formulaire est redisplayed avec un message d’erreur informatif qui les guide pour le fixer. Cela comprend les cas où un utilisateur final affiche une entrée incorrecte (par exemple : une chaîne de date mal formée), ainsi que les cas où le format d’entrée est valide, mais il y a une violation de la règle d’entreprise. Lorsque des erreurs se produisent, le formulaire doit préserver les données d’entrée que l’utilisateur a saisies à l’origine afin qu’ils n’aient pas à remplir leurs modifications manuellement. Ce processus devrait se répéter autant de fois que nécessaire jusqu’à ce que le formulaire se termine avec succès.

ASP.NET MVC comprend quelques belles fonctionnalités intégrées qui rendent la manipulation des erreurs et la forme redisplay facile. Pour voir ces fonctionnalités en action, mettons à jour notre méthode d’action Edit avec le code suivant :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Le code ci-dessus est similaire à notre implémentation précédente - sauf que nous sommes maintenant enveloppant un bloc de manipulation d’erreur d’essai / capture autour de notre travail. Si une exception se produit soit lors de l’appel UpdateModel(), ou lorsque nous essayons de sauver le DinnerRepository (qui soulèvera une exception si l’objet Dîner que nous essayons d’enregistrer est invalide en raison d’une violation de la règle dans notre modèle), notre bloc de traitement des erreurs de capture s’exécutera. En son sein, nous boucle sur toutes les violations de règles qui existent dans l’objet Dîner et les ajouter à un objet ModelState (dont nous discuterons sous peu). Nous redisjouons ensuite la vue.

Pour voir ce travail, réédition de l’application, éditer un dîner, et la changer pour avoir un titre vide, un EventDate de "BOGUS", et utiliser un numéro de téléphone au Royaume-Uni avec une valeur de pays des États-Unis. Lorsque nous appuyons sur le bouton "Enregistrer", notre méthode HTTP POST Edit ne sera pas en mesure d’enregistrer le dîner (parce qu’il y a des erreurs) et redisjouera le formulaire :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Notre application a une expérience d’erreur décente. Les éléments texte avec l’entrée invalide sont mis en surbrillance en rouge, et des messages d’erreur de validation sont affichés à l’utilisateur final à leur sujet. Le formulaire préserve également les données d’entrée que l’utilisateur a saisies à l’origine, de sorte qu’ils n’ont pas à remplir quoi que ce soit.

Comment, vous pourriez demander, cela s’est-il produit? Comment les boîtes à texte Titre, EventDate et ContactPhone se sont-elles mis en évidence en rouge et savent-elles obtenir les valeurs utilisateur entrées à l’origine ? Et comment les messages d’erreur se sont-ils affichés dans la liste en haut ? Les bonnes nouvelles sont que cela ne s’est pas produit par magie - c’est plutôt parce que nous avons utilisé certaines des fonctionnalités intégrées ASP.NET MVC qui rendent la validation des entrées et les scénarios de manipulation des erreurs facile.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Comprendre ModelState et la validation DES méthodes d’aide HTML

Les classes de contrôleur ont une collection de propriétés " ModelState " qui fournit un moyen d’indiquer que des erreurs existent avec un objet modèle étant passé à une vue. Les entrées d’erreur dans la collection ModelState identifient le nom de la propriété modèle avec le problème (par exemple : « Titre », « EventDate » ou « ContactPhone »), et permettent de préciser un message d’erreur respectueux de l’homme (par exemple : « Le titre est requis »).

La méthode *d’aide UpdateModel()* remplit automatiquement la collection ModelState lorsqu’elle rencontre des erreurs tout en essayant d’attribuer des valeurs de formulaire aux propriétés de l’objet modèle. Par exemple, la propriété EventDate de notre objet Dinner est de type DateTime. Lorsque la méthode UpdateModel() n’a pas été en mesure d’attribuer la valeur de chaîne "BOGUS" dans le scénario ci-dessus, la méthode UpdateModel() a ajouté une entrée à la collection ModelState indiquant qu’une erreur d’affectation s’était produite avec cette propriété.

Les développeurs peuvent également écrire du code pour ajouter explicitement des entrées d’erreur dans la collection ModelState comme nous le faisons ci-dessous dans notre bloc de manipulation d’erreurs "catch", qui remplit la collection ModelState avec des entrées basées sur les violations actives des règles dans l’objet Dinner:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Intégration d’aide de html avec ModelState

Les méthodes d’aide HTML - comme Html.TextBox() - vérifient la collection ModelState lors du rendu de la sortie. Si une erreur pour l’article existe, ils rendent la valeur saisie par l’utilisateur et une classe d’erreur CSS.

Par exemple, dans notre vue "Edit", nous utilisons la méthode d’assistance Html.TextBox() pour rendre l’EventDate de notre objet Dinner:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Lorsque la vue a été rendue dans le scénario d’erreur, la méthode Html.TextBox() a vérifié la collection ModelState pour voir s’il y avait des erreurs associées à la propriété "EventDate" de notre objet Dinner. Lorsqu’elle a déterminé qu’il y avait une erreur, elle a rendu l’entrée de l’utilisateur soumis (« BOGUS ») comme valeur, et a ajouté une classe d’erreur css au type &lt;d’entrée « boîte à texte »/&gt; balisage qu’il a généré :

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Vous pouvez personnaliser l’apparence de la classe d’erreur css à regarder comme vous voulez. La classe d’erreur CSS par défaut, « erreur de validation-validation d’entrée », est définie dans la feuille de style *« content.site.css* » et ressemble ci-dessous :

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Cette règle du CSS est ce qui a fait en évidence nos éléments d’entrée invalides ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage () Méthode d’aide

La méthode d’assistance Html.ValidationMessage () peut être utilisée pour produire le message d’erreur ModelState associé à une propriété modèle particulière :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Les sorties de code ci-dessus : * &lt;classe&gt; de portée « erreur&lt;de validation de champ » La valeur ' BOGUS' est invalide/span&gt;*

La méthode d’assistance Html.ValidationMessage() prend également en charge un deuxième paramètre qui permet aux développeurs de passer outre au message texte d’erreur affiché :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Les sorties de code ci-dessus : * &lt;span&gt;\*&lt;classMD&gt; "field-validation-error" /span* instead of the default error text when an error is present for the EventDate property.

##### <a name="htmlvalidationsummary-helper-method"></a>Méthode d’aide Html.ValidationSummary()

La méthode d’assistance Html.ValidationSummary() peut être utilisée pour rendre &lt;&gt;&lt;un&gt;&lt;message&gt; d’erreur sommaire, accompagné d’une liste ul li/ /ul de tous les messages d’erreur détaillés de la collection ModelState :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

La méthode d’assistance Html.ValidationSummary() prend un paramètre de chaîne optionnel, qui définit un message d’erreur sommaire pour afficher au-dessus de la liste des erreurs détaillées :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Vous pouvez utiliser par option CSS pour remplacer à quoi ressemble la liste d’erreurs.

#### <a name="using-a-addruleviolations-helper-method"></a>Utilisation d’une méthode d’aide AddRuleViolations

Notre mise en œuvre initiale de HTTP-POST Edit a utilisé une déclaration de premier coup dans son bloc de capture pour boucler sur les violations des règles de l’objet Dinner et les ajouter à la collection ModelState du contrôleur :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Nous pouvons rendre ce code un peu plus propre en ajoutant une classe "ControllerHelpers" au projet NerdDinner, et mettre en œuvre une méthode d’extension "AddRuleViolations" en elle qui ajoute une méthode d’aide à la classe ASP.NET ModelStateDictionary. Cette méthode d’extension peut résumer la logique nécessaire pour peupler le ModelStateDictionary avec une liste d’erreurs de RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Nous pouvons ensuite mettre à jour notre méthode d’action HTTP-POST Edit pour utiliser cette méthode d’extension pour remplir la collection ModelState avec nos violations des règles du dîner.

#### <a name="complete-edit-action-method-implementations"></a>Mise en œuvre complète de la méthode d’action d’édition

Le code ci-dessous implémente toute la logique du contrôleur nécessaire à notre scénario Edit :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

La bonne chose au sujet de notre mise en œuvre Edit est que ni notre classe Controller ni notre modèle De vue n’a à savoir quoi que ce soit sur la validation spécifique ou les règles d’affaires appliquées par notre modèle de dîner. Nous pouvons ajouter des règles supplémentaires à notre modèle à l’avenir et n’avons pas à apporter de modifications de code à notre contrôleur ou à notre vue pour qu’elles soient prises en charge. Cela nous donne la flexibilité de faire évoluer facilement nos exigences d’application à l’avenir avec un minimum de modifications de code.

### <a name="create-support"></a>Créer un support

Nous avons fini de mettre en œuvre le comportement "Edit" de notre classe DinnersController. Passons maintenant à la mise en œuvre du support "Créer" sur elle - qui permettra aux utilisateurs d’ajouter de nouveaux dîners.

#### <a name="the-http-get-create-action-method"></a>La méthode HTTP-GET Créer de l’action

Nous allons commencer par mettre en œuvre le comportement HTTP "GET" de notre méthode d’action créer. Cette méthode sera appelée lorsque quelqu’un visite l’URL */Dîners/Créer.* Notre implémentation ressemble à :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Le code ci-dessus crée un nouvel objet De dîner, et assigne sa propriété EventDate à être une semaine à l’avenir. Il rend ensuite une vue qui est basée sur le nouvel objet Dinner. Parce que nous n’avons pas explicitement passé un nom à la *méthode d’aide View(),* il utilisera le chemin par défaut basé sur la convention pour résoudre le modèle de vue: /Vues /Dîners/Create.aspx.

Créons maintenant ce modèle de vue. Nous pouvons le faire en cliquant à droite dans la méthode d’action Créer et en sélectionnant la commande de menu contextuelle « Add View ». Dans le dialogue "Add View", nous indiquerons que nous passons un objet de dîner au modèle de vue, et choisirons d’auto-échafauder un modèle "Créer":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Lorsque nous cliquez sur le bouton "Ajouter", Visual Studio enregistrera une nouvelle vue "Create.aspx" à base d’échafaudages sur le répertoire "Views-Dîners" et l’ouvrira dans l’IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Faisons quelques modifications au fichier par défaut d’échafaudage qui a été généré pour nous, et modifions-le pour ressembler ci-dessous :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

Et maintenant, lorsque nous exécuteons notre application et accédons à l’URL *"/Dîners/Créer"* dans le navigateur, il rendra l’interface utilisateur comme ci-dessous à partir de notre implémentation d’action Créer:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Mise en œuvre de la méthode d’action HTTP-POST Créer

Nous avons mis en œuvre la version HTTP-GET de notre méthode d’action Create. Lorsqu’un utilisateur clique sur le bouton «Enregistrer», il effectue un formulaire à l’URL&gt; */Dîners/Créer,* et soumet les valeurs du formulaire d’entrée HTML &lt;à l’aide du verbe HTTP POST.

Mettons maintenant en œuvre le comportement HTTP POST de notre méthode d’action de création. Nous allons commencer par ajouter une méthode d’action surchargée "Créer" à notre DinnersController qui a un "AcceptVerbs" attribut sur elle qui indique qu’il gère les scénarios HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Il existe une variété de façons d’accéder aux paramètres de formulaire affichés dans notre méthode HTTP-POST activée "Créer".

Une approche consiste à créer un nouvel objet De dîner, puis à utiliser la méthode *d’aide UpdateModel (comme* nous l’avons fait avec l’action Edit) pour le remplir avec les valeurs de formulaire affichées. Nous pouvons ensuite l’ajouter à notre DinnerRepository, le persister vers la base de données, et rediriger l’utilisateur vers notre action Détails pour montrer le dîner nouvellement créé en utilisant le code ci-dessous:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternativement, nous pouvons utiliser une approche où nous avons notre méthode d’action Créer () prendre un objet de dîner comme un paramètre de méthode. ASP.NET MVC va alors automatiquement instantané un nouvel objet de dîner pour nous, remplir ses propriétés en utilisant les entrées de formulaire, et le transmettre à notre méthode d’action:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Notre méthode d’action ci-dessus vérifie que l’objet Dinner a été peuplé avec succès avec les valeurs de poteau de forme en vérifiant la propriété ModelState.IsValid. Cela reviendra faux s’il y a des problèmes de conversion d’entrée (par exemple : une chaîne de "BOGUS" pour la propriété EventDate), et s’il y a des problèmes, notre méthode d’action redisjoue le formulaire.

Si les valeurs d’entrée sont valides, alors la méthode d’action tente d’ajouter et d’enregistrer le nouveau dîner au DinnerRepository. Il enveloppe ce travail dans un bloc d’essai/capture et redisplays le formulaire s’il ya des violations des règles d’affaires (ce qui causerait le dinnerRepository.Save() méthode de soulever une exception).

Pour voir ce comportement de manipulation d’erreur en action, nous pouvons demander l’URL */Dîners/Créer* et remplir des détails sur un nouveau dîner. Une entrée ou des valeurs incorrectes fera redessmer le formulaire de création avec les erreurs mises en évidence comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Remarquez comment notre formulaire Create honore exactement les mêmes règles de validation et d’affaires que notre formulaire Edit. C’est parce que nos règles de validation et d’entreprise ont été définies dans le modèle, et n’ont pas été intégrés dans l’interface utilisateur ou le contrôleur de l’application. Cela signifie que nous pouvons plus tard modifier /évoluer nos règles de validation ou d’affaires en un seul endroit et les faire appliquer tout au long de notre application. Nous n’aurons pas à modifier de code dans nos méthodes d’action Edit ou Créer pour honorer automatiquement les nouvelles règles ou modifications apportées aux règles existantes.

Lorsque nous réparons les valeurs d’entrée et cliquez sur le bouton «Enregistrer» à nouveau, notre ajout à la DinnerRepository réussira, et un nouveau dîner sera ajouté à la base de données. Nous serons ensuite redirigés vers l’URL */Dîners/Détails/[id]* où nous serons présentés avec des détails sur le dîner nouvellement créé:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Supprimer le support

Ajoutons maintenant un support « Supprimer » à notre DinnersController.

#### <a name="the-http-get-delete-action-method"></a>La méthode d’action de suppression HTTP-GET

Nous allons commencer par implémenter le comportement HTTP GET de notre méthode d’action supprimer. Cette méthode sera appelée lorsque quelqu’un visite l’URL */Dîners/Supprimer/[id].* Voici la mise en œuvre:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

La méthode d’action tente de récupérer le dîner à supprimer. Si le dîner existe, il rend une vue basée sur l’objet Dîner. Si l’objet n’existe pas (ou a déjà été supprimé), il renvoie une vue qui rend le modèle de vue "NotFound" que nous avons créé plus tôt pour notre méthode d’action "Détails".

Nous pouvons créer le modèle de vue "Supprimer" en cliquant à droite dans la méthode d’action Supprimer et en sélectionnant la commande de menu contextuelle "Add View". Dans le dialogue "Add View", nous indiquerons que nous passons un objet de dîner à notre modèle de vue comme modèle, et choisirons de créer un modèle vide:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Lorsque nous cliquez sur le bouton "Ajouter", Visual Studio ajoutera un nouveau fichier de modèle de vue "Supprimer.aspx" pour nous dans notre répertoire "Views-Dîners". Nous allons ajouter un peu de HTML et de code au modèle pour implémenter un écran de confirmation supprimer comme ci-dessous:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Le code ci-dessus affiche le titre du dîner &lt;à&gt; supprimer, et supprime un élément de formulaire qui fait un POST à l’URL /Dîners/Supprimer/[id] si l’utilisateur final clique sur le bouton "Supprimer" en son sein.

Lorsque nous passons notre application et accédons à l’URL *"/Dîners/Supprimer/[id]"* pour un objet de dîner valide, elle rend l’interface utilisateur comme ci-dessous :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Sujet secondaire: Pourquoi faisons-nous un POST?** |
| --- |
| Vous pourriez demander - pourquoi avons-nous &lt;passé&gt; par l’effort de créer un formulaire dans notre écran de confirmation Supprimer? Pourquoi ne pas simplement utiliser un hyperlien standard pour lier à une méthode d’action qui supprime l’opération réelle? La raison en est que nous voulons être prudents pour se prémunir contre les web-crawlers et les moteurs de recherche de découvrir nos URL et par inadvertance causer des données à supprimer quand ils suivent les liens. LES URL basées sur HTTP-GET sont considérées comme « sûres » pour qu’elles y aient accès/crawl, et elles sont censées ne pas suivre les URL HTTP-POST. Une bonne règle est de s’assurer que vous mettez toujours des opérations destructrices ou de modification de données derrière les demandes HTTP-POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Mise en œuvre de la méthode d’action de suppression HTTP-POST

Nous avons maintenant la version HTTP-GET de notre méthode d’action Supprimer implémentée qui affiche un écran de confirmation de suppression. Lorsqu’un utilisateur final clique sur le bouton «Supprimer», il effectue un formulaire post sur l’URL */Dîner/dîner/[id].*

Implémentons maintenant le comportement HTTP "POST" de la méthode d’action supprimer en utilisant le code ci-dessous:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

La version HTTP-POST de notre méthode d’action Supprimer tente de récupérer l’objet du dîner à supprimer. S’il ne peut pas le trouver (parce qu’il a déjà été supprimé), il rend notre modèle "NotFound". S’il trouve le dîner, il le supprime du DinnerRepository. Il rend ensuite un modèle "supprimé".

Pour implémenter le modèle "Supprimé", nous cliquerons à droite dans la méthode d’action et choisirons le menu contextable "Ajouter la vue". Nous nommerons notre vue "Supprimé" et nous ferons en sorte qu’il s’agit d’un modèle vide (et ne prenons pas un objet modèle fortement typé). Nous y ajouterons ensuite du contenu HTML :

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

Et maintenant, lorsque nous passons notre application et accédons à *l’URL "/Dîners/Supprimer/[id]"* pour un objet de dîner valide, il rendra notre dîner supprimer l’écran de confirmation comme ci-dessous:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Lorsque nous cliquez sur le bouton "Supprimer", il effectuera un HTTP-POST à l’URL */Dîners/Supprimer/[id],* qui supprimera le dîner de notre base de données, et affichera notre modèle de vue " Supprimé " :

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Modèle de sécurité de liaison

Nous avons discuté de deux façons différentes d’utiliser les caractéristiques intégrées de liaison de modèle de ASP.NET MVC. La première utilisant la méthode UpdateModel() pour mettre à jour les propriétés sur un objet modèle existant, et la seconde en utilisant ASP.NET support de MVC pour passer des objets modèles en tant que paramètres de méthode d’action. Ces deux techniques sont très puissantes et extrêmement utiles.

Ce pouvoir apporte également avec lui la responsabilité. Il est important d’être toujours paranoïaque au sujet de la sécurité lors de l’acceptation de toute entrée utilisateur, et cela est également vrai lorsque les objets de liaison pour former des entrées. Vous devez faire attention à toujours coder les valeurs entrantes par l’utilisateur pour éviter les attaques d’injection HTML et JavaScript, et faire attention aux attaques d’injection SQL (note : nous utilisons LINQ à SQL pour notre application, qui code automatiquement les paramètres pour prévenir ces types d’attaques). Vous ne devriez jamais compter uniquement sur la validation côté client, et toujours utiliser la validation côté serveur pour vous protéger contre les pirates qui tentent de vous envoyer de fausses valeurs.

Un élément de sécurité supplémentaire pour vous assurer que vous pensez lors de l’utilisation des caractéristiques de liaison de ASP.NET MVC est la portée des objets que vous reliez. Plus précisément, vous voulez vous assurer que vous comprenez les implications de sécurité des propriétés que vous permettez d’être lié, et assurez-vous que vous ne permettez que les propriétés qui devraient vraiment être updatable par un utilisateur final d’être mis à jour.

Par défaut, la méthode UpdateModel() tentera de mettre à jour toutes les propriétés sur l’objet modèle qui correspondent aux valeurs de paramètres de formulaire entrantes. De même, les objets passés comme paramètres de méthode d’action aussi par défaut peuvent avoir toutes leurs propriétés définies via des paramètres de forme.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Verrouillage de la liaison par utilisation

Vous pouvez verrouiller la politique contraignante sur une base par utilisation en fournissant une «liste d’inclure» explicite des propriétés qui peuvent être mises à jour. Cela peut être fait en passant un paramètre de tableau de chaîne supplémentaire à la méthode UpdateModel() comme ci-dessous:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Les objets adoptés sous forme de paramètres de méthode d’action prennent également en charge un attribut [Bind] qui permet de préciser une « liste d’éléments » des propriétés autorisées comme ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Verrouillage de la liaison sur une base type

Vous pouvez également verrouiller les règles contraignantes par type. Cela vous permet de spécifier les règles contraignantes une fois, puis de les faire appliquer dans tous les scénarios (y compris les scénarios de paramètres de mise à jour et de méthode d’action) entre tous les contrôleurs et les méthodes d’action.

Vous pouvez personnaliser les règles de liaison par type en ajoutant un attribut [Bind] sur un type, ou en l’enregistrant dans le fichier Global.asax de l’application (utile pour les scénarios où vous ne possédez pas le type). Vous pouvez ensuite utiliser les propriétés Inclure et exclure l’attribut Bind pour contrôler quelles propriétés sont li bindables pour la classe ou l’interface particulière.

Nous allons utiliser cette technique pour la classe Dîner dans notre application NerdDinner, et ajouter un attribut [Bind] à elle qui limite la liste des propriétés liantes à ce qui suit:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Notez que nous ne permettons pas que la collection RSVPs soit manipulée par liaison, et nous ne permettons pas non plus que les propriétés DinnerID ou HostedBy soient définies via la reliure. Pour des raisons de sécurité, nous ne manipulerons ces propriétés particulières qu’en utilisant du code explicite dans nos méthodes d’action.

### <a name="crud-wrap-up"></a>Emballage CRUD

ASP.NET MVC comprend un certain nombre de fonctionnalités intégrées qui aident à mettre en œuvre des scénarios d’affichage des formulaires. Nous avons utilisé une variété de ces fonctionnalités pour fournir un support d’interface utilisateur CRUD en plus de notre DinnerRepository.

Nous utilisons une approche axée sur les modèles pour mettre en œuvre notre application. Cela signifie que toute notre logique de validation et de règle d’entreprise est définie dans notre couche de modèle - et non dans nos contrôleurs ou vues. Ni notre classe Controller ni nos modèles View ne savent quoi que ce soit sur les règles commerciales spécifiques appliquées par notre classe de modèle De dîner.

Cela permettra de garder notre architecture d’application propre et de le rendre plus facile à tester. Nous pouvons ajouter des règles d’affaires supplémentaires à notre couche de modèle à l’avenir et *ne pas avoir à apporter de modifications de code* à notre contrôleur ou à notre vue pour qu’elles soient prises en charge. Cela va nous fournir beaucoup d’agilité pour évoluer et changer notre application à l’avenir.

Notre DinnersController permet maintenant des listes/détails de dîner, ainsi que de créer, modifier et supprimer le support. Le code complet de la classe peut être trouvé ci-dessous :

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>étape suivante

Nous avons maintenant de base CRUD (Créer, lire, mettre à jour et supprimer) mise en œuvre dans notre classe DinnersController.

Examinons maintenant comment nous pouvons utiliser les classes ViewData et ViewModel pour permettre une interface utilisateur encore plus riche sur nos formulaires.

> [!div class="step-by-step"]
> [Suivant précédent](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Next](use-viewdata-and-implement-viewmodel-classes.md)
