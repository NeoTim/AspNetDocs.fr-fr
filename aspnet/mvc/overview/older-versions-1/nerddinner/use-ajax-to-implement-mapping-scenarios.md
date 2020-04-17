---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Utilisez AJAX pour mettre en œuvre des scénarios de cartographie .fr Microsoft Docs
author: rick-anderson
description: L’étape 11 montre comment intégrer le support cartographique AJAX dans notre application NerdDinner, permettant aux utilisateurs qui créent, éditent ou regardent des dîners de voir le...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f2e2640eb421d5ee8006915f46cbe1090b8d21ad
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542584"
---
# <a name="use-ajax-to-implement-mapping-scenarios"></a>Utiliser AJAX pour implémenter des scénarios de mappage

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 11 d’un tutoriel gratuit ["NerdDinner" application](introducing-the-nerddinner-tutorial.md) qui marche à travers la façon de construire une petite, mais complète, application web en utilisant ASP.NET MVC 1.
> 
> L’étape 11 montre comment intégrer le support cartographique AJAX dans notre application NerdDinner, permettant aux utilisateurs qui créent, éditent ou regardent des dîners de voir graphiquement l’emplacement du dîner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les tutoriels [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [MVC Music Store.](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)

## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner Step 11: Intégration d’une carte AJAX

Nous allons maintenant rendre notre application un peu plus visuellement excitante en intégrant le support de cartographie AJAX. Cela permettra aux utilisateurs qui créent, éditent ou regardent des dîners de voir l’emplacement du dîner graphiquement.

### <a name="creating-a-map-partial-view"></a>Création d’une carte Vue partielle

Nous allons utiliser la fonctionnalité de cartographie à plusieurs endroits de notre application. Pour conserver notre code DRY, nous encapsulerons la fonctionnalité de la carte commune dans un seul modèle partiel que nous pouvons réutilisation à travers plusieurs actions et vues de contrôleur. Nous nommerons cette vue partielle "map.ascx" et la créerons dans le répertoire "Views".

Nous pouvons créer la carte.ascx partielle en cliquant à droite sur le&gt;répertoire «Vues-Dîners» et en choisissant la commande de menu Add-View. Nous allons nommer la vue "Map.ascx", le vérifier comme une vue partielle, et indiquer que nous allons le passer une classe de modèle "Dîner" fortement typée:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Lorsque nous clions sur le bouton "Ajouter", notre modèle partiel sera créé. Nous mettrons ensuite à jour le fichier Map.ascx pour disposer du contenu suivant :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

La &lt;première&gt; référence de script indique la bibliothèque de cartographie Microsoft Virtual Earth 6.2. La &lt;deuxième&gt; référence de script indique un fichier map.js que nous allons bientôt créer qui encapsulera notre logique commune de cartographie Javascript. L’élément &lt;div id"theMap"&gt; est le conteneur HTML que la Terre Virtuelle utilisera pour héberger la carte.

Nous avons ensuite &lt;&gt; un bloc de script intégré qui contient deux fonctions JavaScript spécifiques à cette vue. La première fonction utilise jQuery pour filer une fonction qui s’exécute lorsque la page est prête à exécuter le script côté client. Il appelle une fonction d’aide LoadMap() que nous définirons dans notre fichier de script Map.js pour charger le contrôle de la carte de la terre virtuelle. La deuxième fonction est un gestionnaire d’événements de rappel qui ajoute une épingle à la carte qui identifie un emplacement.

Remarquez comment nous utilisons &lt;un bloc&gt; de % du côté du serveur dans le bloc de script côté client pour intégrer la latitude et la longitude du dîner que nous voulons cartographier dans le JavaScript. Il s’agit d’une technique utile pour produire des valeurs dynamiques qui peuvent être utilisées par le script côté client (sans nécessiter un rappel AJAX séparé vers le serveur pour récupérer les valeurs - ce qui le rend plus rapide). Les &lt;blocs&gt; de % à % s’exécuteront lorsque la vue est de rendu sur le serveur - et ainsi la sortie du HTML se retrouvera juste avec des valeurs JavaScript intégrées (par exemple : latitude var - 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Création d’une bibliothèque d’utilité Map.js

Créons maintenant le fichier Map.js que nous pouvons utiliser pour encapsuler la fonctionnalité JavaScript pour notre carte (et implémenter les méthodes LoadMap et LoadPin ci-dessus). Nous pouvons le faire en cliquant à droite sur l’annuaire des scripts&gt;dans notre projet, puis choisir la commande de menu "Ajouter- Nouvel élément", sélectionner l’élément JScript, et le nommer "Map.js".

Voici le code JavaScript que nous allons ajouter au fichier Map.js qui interagira avec la Terre virtuelle pour afficher notre carte et y ajouter des épingles pour nos dîners :

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Intégration de la carte avec des formulaires de création et d’édition

Nous allons maintenant intégrer le support Map à nos scénarios créer et modifier existants. La bonne nouvelle, c’est que c’est assez facile à faire et ne nous oblige pas à modifier l’un de nos codes Controller. Parce que nos vues Créer et modifier partagent une vue partielle commune "DinnerForm" pour implémenter l’interface utilisateur forme de dîner, nous pouvons ajouter la carte en un seul endroit et avoir à la fois nos scénarios Créer et modifier l’utiliser.

Tout ce que nous devons faire est d’ouvrir la vue partielle «Views-Dîners-DinnerForm.ascx» et de le mettre à jour pour inclure notre nouvelle carte partielle. Voici ce que le DinnerForm mis à jour ressemblera une fois la carte est ajoutée (note: les éléments de formulaire HTML sont omis de l’extrait de code ci-dessous pour la brièveté):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Le DinnerForm partiel ci-dessus prend un objet de type "DinnerFormViewModel" comme son type de modèle (parce qu’il a besoin à la fois d’un objet de dîner, ainsi que d’une SelectList pour remplir la liste de décrocheurs des pays). Notre carte partielle a juste besoin d’un objet de type "Dîner" comme son type de modèle, et donc quand nous rendons la carte partielle, nous passons juste le dîner sous-propriété de DinnerFormViewModel à elle:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La fonction JavaScript que nous avons ajoutée à la jQuery utilise partiellement pour joindre un événement "blur" à la boîte de texte HTML "Adresse". Vous avez probablement entendu parler d’événements « focus » qui s’enflammer lorsqu’un utilisateur clique ou fait un onglet dans une boîte à texte. Le contraire est un événement "blur" qui s’allume lorsqu’un utilisateur sort d’une boîte de texte. Le gestionnaire d’événements ci-dessus efface les valeurs de la boîte de texte de latitude et de longitude lorsque cela se produit, puis trace le nouvel emplacement de l’adresse sur notre carte. Un gestionnaire d’événements de rappel que nous avons défini dans le fichier map.js mettra ensuite à jour les boîtes de texte de longitude et de latitude sur notre formulaire en utilisant des valeurs retournées par la terre virtuelle en fonction de l’adresse que nous lui avons donnée.

Et maintenant, lorsque nous exéduons notre application à nouveau et cliquez sur l’onglet "Dîner hôte", nous verrons une carte par défaut affichée avec nos éléments de formulaire dîner standard:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Lorsque nous tapons une adresse, puis onglet loin, la carte sera dynamiquement mise à jour pour afficher l’emplacement, et notre gestionnaire d’événement remplira les boîtes de texte latitude / longitude avec les valeurs de localisation:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Si nous économisons le nouveau dîner et l’ouvrons à nouveau pour l’édition, nous constaterons que l’emplacement de la carte est affiché lorsque la page se charge:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Chaque fois que le champ d’adresse est changé, la carte et les coordonnées latitude/longitude seront mises à jour.

Maintenant que la carte affiche l’emplacement du dîner, nous pouvons également modifier les champs de forme Latitude et Longitude d’être des boîtes de texte visibles à la place d’être des éléments cachés (puisque la carte est automatiquement les mettre à jour chaque fois qu’une adresse est saisie). Pour ce faire, nous passerons de l’utilisation de l’aide HTML.TextBox() HTML à l’aide de la méthode d’aide Html.Hidden() :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Et maintenant, nos formulaires sont un peu plus convivial et éviter d’afficher la latitude brute / longitude (tout en les stockant à chaque dîner dans la base de données):

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Intégration de la carte avec la vue des détails

Maintenant que la carte est intégrée à nos scénarios Créer et modifier, insédons-la également à notre scénario Détails. Tout ce que nous devons &lt;faire est d’appeler % Html.RenderPartial ("carte"); %&gt; dans la vue Détails.

Voici ce que le code source à la vue complète Détails (avec l’intégration de la carte) ressemble:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Et maintenant, quand un utilisateur navigue à un /Dîners / Détails / [id] URL, ils vont voir les détails sur le dîner, l’emplacement du dîner sur la carte (complet avec un push-pin qui, lorsqu’il est plané sur affiche le titre du dîner et l’adresse de celui-ci), et ont un lien AJAX à RSVP pour elle:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Mise en œuvre de la recherche de localisation dans notre base de données et notre dépôt

Pour terminer notre implémentation AJAX, ajoutons une carte à la page d’accueil de l’application qui permet aux utilisateurs de rechercher graphiquement des dîners près d’eux.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Nous commencerons par mettre en œuvre le support dans notre base de données et couche de dépôt de données pour effectuer efficacement une recherche de rayon basée sur l’emplacement pour les dîners. Nous pourrions utiliser les nouvelles [caractéristiques géospatiales de SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) pour mettre en œuvre cette, [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) ou bien nous pouvons utiliser une approche de fonction SQL que Gary Dryden a discuté dans l’article ici: et Rob Conery blogué sur l’utilisation avec LINQ à SQL ici:[http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Pour mettre en œuvre cette technique, nous ouvrirons le "Server Explorer" au sein de Visual Studio, sélectionnerons la base de données NerdDinner, puis cliquez à droite sur le sous-nœud "fonctions" en dessous et choisirons de créer une nouvelle "fonction de valeur Scalaire":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Nous allons ensuite coller dans la fonction suivante DistanceBetween:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Nous allons ensuite créer une nouvelle fonction de table-valeur dans SQL Server que nous appellerons "NearestDinners":

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Cette fonction de table "NearestDinners" utilise la fonction d’aide DistanceBetween pour retourner tous les dîners à moins de 100 miles de la latitude et de la longitude que nous lui fournissons:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Pour appeler cette fonction, nous allons d’abord ouvrir le LINQ au concepteur SQL en faisant double cliquant sur le fichier NerdDinner.dbml dans notre répertoire 'Models:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Nous allons ensuite faire glisser les fonctions NearestDinners et DistanceBetween sur le LINQ au concepteur SQL, ce qui les fera ajouter comme méthodes sur notre LINQ à SQL NerdDinnerDataContext classe:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Nous pouvons alors exposer une méthode de requête "FindByLocation" sur notre classe DinnerRepository qui utilise la fonction NearestDinner pour retourner les dîners à venir qui sont à moins de 100 miles de l’emplacement spécifié:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Mise en œuvre d’une méthode d’action de recherche AJAX basée sur JSON

Nous allons maintenant mettre en œuvre une méthode d’action de contrôleur qui tire parti de la nouvelle méthode de dépôt FindByLocation() pour revenir sur une liste de données de dîner qui peuvent être utilisées pour remplir une carte. Nous aurons cette méthode d’action retourner les données du dîner dans un format JSON (JavaScript Object Notation) afin qu’il puisse être facilement manipulé en utilisant JavaScript sur le client.

Pour mettre en œuvre cela, nous allons créer une nouvelle classe "SearchController" en&gt;cliquant à droite sur le répertoire des contrôleurs et en choisissant la commande de menu Add-Controller. Nous metndons ensuite en œuvre une méthode d’action "SearchByLocation" dans la nouvelle classe SearchController comme ci-dessous:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

La méthode d’action SearchByLocation du SearchController appelle en interne la méthode FindByLocation sur DinnerRepository pour obtenir une liste des dîners à proximité. Plutôt que de retourner les objets du dîner directement au client, cependant, il retourne plutôt les objets JsonDinner. La classe JsonDinner expose un sous-ensemble de propriétés dîner (par exemple: pour des raisons de sécurité, il ne divulgue pas les noms des personnes qui ont RSVP’d pour un dîner). Il comprend également une propriété RSVPCount qui n’existe pas sur Le dîner et qui est calculée dynamiquement en comptant le nombre d’objets RSVP associés à un dîner particulier.

Nous utilisons ensuite la méthode d’aide Json() sur la classe de base Controller pour retourner la séquence des dîners à l’aide d’un format fil basé sur JSON. JSON est un format texte standard pour représenter des structures de données simples. Voici un exemple de ce qu’est une liste formatée JSON de deux objets JsonDinner ressemble à son retour de notre méthode d’action:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Appel de la méthode AJAX basée sur JSON à l’aide de jQuery

Nous sommes maintenant prêts à mettre à jour la page d’accueil de l’application NerdDinner pour utiliser la méthode d’action SearchByLocation du SearchController. Pour ce faire, nous allons ouvrir le /Views /Home/Index.aspx voir le modèle et le mettre &lt;à&gt; jour pour avoir une boîte texte, bouton de recherche, notre carte, et un élément div nommé dinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Nous pouvons ensuite ajouter deux fonctions JavaScript à la page :

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La première fonction JavaScript charge la carte lorsque la page se charge. La deuxième fonction JavaScript file un gestionnaire d’événements JavaScript cliquez sur le bouton de recherche. Lorsque le bouton est appuyé, il appelle la fonction FindDinnersGivenLocation() JavaScript que nous ajouterons à notre fichier Map.js :

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Cette fonction FindDinnersGivenLocation () appelle la carte. Trouvez() sur le contrôle virtuel de la Terre pour le centrer sur l’emplacement entré. Lorsque le service de carte de la terre virtuelle revient, la carte. Trouver() méthode invoque la méthode de rappel callbackUpdateMapDinners nous l’avons passé comme l’argument final.

La méthode callbackUpdateMapDinners() est l’endroit où le vrai travail est fait. Il utilise la méthode d’aide de jQuery $.post() pour effectuer un appel AJAX à notre SearchController SearchByLocation () méthode d’action - en lui passant la latitude et la longitude de la carte nouvellement centrée. Il définit une fonction en ligne qui sera appelée lorsque la méthode d’aide $.post() se termine, et les résultats du dîner formaté JSON retournés de la méthode d’action SearchByLocation() seront adoptés à l’aide d’une variable appelée «dîners». Il fait ensuite un avant-plan sur chaque dîner retourné, et utilise la latitude du dîner et la longitude et d’autres propriétés pour ajouter une nouvelle broche sur la carte. Il ajoute également une entrée de dîner à la liste HTML des dîners à droite de la carte. Il a ensuite fil-up un événement de vol stationnaire à la fois pour les pushpins et la liste HTML de sorte que les détails sur le dîner sont affichés quand un utilisateur plane au-dessus d’eux:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Et maintenant, lorsque nous exécuterons l’application et visiterons la page d’accueil, nous serons présentés avec une carte. Lorsque nous entrons dans le nom d’une ville, la carte affichera les dîners à venir près de lui:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Planer au-dessus d’un dîner affichera des détails à ce sujet.

En cliquant sur le titre du dîner, que ce soit dans la bulle ou sur le côté droit dans la liste HTML nous naviguera vers le dîner - que nous pouvons ensuite optionnellement RSVP pour:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>étape suivante

Nous avons maintenant implémenté toutes les fonctionnalités d’application de notre application NerdDinner. Examinons maintenant comment nous pouvons permettre des tests unitaires automatisés.

> [!div class="step-by-step"]
> [Suivant précédent](use-ajax-to-deliver-dynamic-updates.md)
> [Next](enable-automated-unit-testing.md)
