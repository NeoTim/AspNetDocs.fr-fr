---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
title: Transmettre des données pour afficher les pages maîtresses (VB) Microsoft Docs
author: rick-anderson
description: Le but de ce tutoriel est d’expliquer comment vous pouvez passer les données d’un contrôleur à une page principale de vue. Nous examinons deux stratégies pour transmettre des données à une vue m...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: 37a1ebae-8773-408f-8645-d21da7ff9ae1
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: ef65541a5f2297414f923e2f8108a14de67531e5
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542467"
---
# <a name="passing-data-to-view-master-pages-vb"></a>Passage de données à des pages maîtres de vue (VB)

par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_VB.pdf)

> Le but de ce tutoriel est d’expliquer comment vous pouvez passer les données d’un contrôleur à une page principale de vue. Nous examinons deux stratégies pour transmettre les données à une page maîtresse de vue. Tout d’abord, nous discutons d’une solution facile qui se traduit par une application qui est difficile à maintenir. Ensuite, nous examinons une bien meilleure solution qui nécessite un peu plus de travail initial, mais aboutit à une application beaucoup plus maintenable.

## <a name="passing-data-to-view-master-pages"></a>Transmettre des données pour afficher les pages maîtresses

Le but de ce tutoriel est d’expliquer comment vous pouvez passer les données d’un contrôleur à une page principale de vue. Nous examinons deux stratégies pour transmettre les données à une page maîtresse de vue. Tout d’abord, nous discutons d’une solution facile qui se traduit par une application qui est difficile à maintenir. Ensuite, nous examinons une bien meilleure solution qui nécessite un peu plus de travail initial, mais aboutit à une application beaucoup plus maintenable.

### <a name="the-problem"></a>Le problème

Imaginez que vous construisez une application de base de données de films et que vous souhaitez afficher la liste des catégories de films sur chaque page de votre application (voir la figure 1). Imaginez, en outre, que la liste des catégories de films est stockée dans un tableau de base de données. Dans ce cas, il serait logique de récupérer les catégories de la base de données et de rendre la liste des catégories de films dans une page principale de vue.

[![Affichage des catégories de films dans une page maîtresse de vue](passing-data-to-view-master-pages-vb/_static/image2.png)](passing-data-to-view-master-pages-vb/_static/image1.png)

**Figure 01**: Affichage des catégories de films dans une page principale de vue ([Cliquez pour voir l’image grandeur nature](passing-data-to-view-master-pages-vb/_static/image3.png))

Voilà le problème. Comment récupérer la liste des catégories de films dans la page principale ? Il est tentant d’appeler directement les méthodes de vos classes modèles dans la page principale. En d’autres termes, il est tentant d’inclure le code pour récupérer les données de la base de données directement dans votre page principale. Cependant, le contournement de vos contrôleurs MVC pour accéder à la base de données violerait la séparation propre des préoccupations qui est l’un des principaux avantages de la construction d’une application MVC.

Dans une application MVC, vous souhaitez que toute interaction entre vos vues MVC et votre modèle MVC soit gérée par vos contrôleurs MVC. Cette séparation des préoccupations se traduit par une application plus maintenable, adaptable et testable.

Dans une application MVC, toutes les données transmises à une vue , y compris une page maîtresse de vue, doivent être transmises à une vue par une action de contrôleur. En outre, les données doivent être transmises en tirant parti des données de vue. Dans le reste de ce tutoriel, j’examine deux méthodes de transmission des données de vue à une page principale de vue.

### <a name="the-simple-solution"></a>La solution simple

Commençons par la solution la plus simple pour transmettre les données de vue d’un contrôleur à une page principale de vue. La solution la plus simple est de passer les données de vue pour la page principale dans chaque action de contrôleur.

Considérez le contrôleur dans la liste 1. Il expose deux `Index()` actions `Details()`nommées et . La `Index()` méthode d’action renvoie chaque film dans la table de base de données Movies. La `Details()` méthode d’action retourne chaque film dans une catégorie de film particulier.

**Liste 1`Controllers\HomeController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample1.vb)]

Notez que `Index()` les `Details()` deux et les actions ajoutent deux éléments pour afficher les données. L’action `Index()` ajoute deux clés : les catégories et les films. La clé des catégories représente la liste des catégories de films affichées par la page principale de vue. La clé de films représente la liste des films affichés par la page de vue Index.

L’action `Details()` ajoute également deux clés nommées catégories et films. La clé des catégories, une fois de plus, représente la liste des catégories de films affichées par la page principale de vue. La clé de films représente la liste des films dans une catégorie particulière affichée par la page De vue Détails (voir la figure 2).

[![La vue Détails](passing-data-to-view-master-pages-vb/_static/image5.png)](passing-data-to-view-master-pages-vb/_static/image4.png)

**Figure 02**: La vue Détails ([Cliquez pour voir l’image grandeur nature](passing-data-to-view-master-pages-vb/_static/image6.png))

La vue de l’indice est contenue dans la liste 2. Il itère simplement à travers la liste des films représentés par l’élément des films en vue des données.

**Liste 2`Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample2.aspx)]

La page principale de vue est contenue dans la liste 3. La page principale de vue itère et rend toutes les catégories de films représentées par l’élément des catégories à partir des données de vue.

**Liste 3`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-vb/samples/sample3.aspx)]

Toutes les données sont transmises à la vue et à la page principale de vue par le biais des données de vue. C’est la bonne façon de transmettre les données à la page principale.

Alors, qu’est-ce qui ne va pas avec cette solution? Le problème est que cette solution viole le principe DRY (Don’t Repeat Yourself). Chaque action de contrôleur doit ajouter la même liste de catégories de films pour afficher les données. Le fait d’avoir du code en double dans votre application rend votre application beaucoup plus difficile à maintenir, à adapter et à modifier.

### <a name="the-good-solution"></a>La bonne solution

Dans cette section, nous examinons une solution alternative, et meilleure, pour transmettre des données d’une action de contrôleur à une page principale de vue. Au lieu d’ajouter les catégories de films pour la page principale dans chaque action de contrôleur, nous ajoutons les catégories de films aux données de vue qu’une seule fois. Toutes les données de vue utilisées par la page principale de vue sont ajoutées dans un contrôleur d’application.

La classe ApplicationController est contenue dans la liste 4.

La classe ApplicationController est contenue dans la liste 4.

**Liste 4`Controllers\ApplicationController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample4.vb)]

Il ya trois choses que vous devriez remarquer sur le contrôleur de demande dans la liste 4. Tout d’abord, notez que la classe hérite de la classe de base System.Web.Mvc.Controller. Le contrôleur d’application est une classe de contrôleur.

Deuxièmement, notez que la classe de contrôleur d’application est une classe MustInherit. Une classe MustInherit est une classe qui doit être mise en œuvre par une classe concrète. Étant donné que le contrôleur d’application est une classe MustInherit, vous ne pouvez pas invoquer directement les méthodes définies dans la classe. Si vous tentez d’invoquer directement la classe d’application, vous recevrez un message d’erreur ne peut pas être trouvé.

Troisièmement, notez que le contrôleur d’application contient un constructeur qui ajoute la liste des catégories de films pour afficher les données. Chaque classe de contrôleur qui hérite du contrôleur d’application appelle automatiquement le constructeur du contrôleur d’application. Chaque fois que vous appelez une action sur n’importe quel contrôleur qui hérite du contrôleur d’application, les catégories de films sont incluses dans les données de vue automatiquement.

Le contrôleur de films dans La liste 5 hérite du contrôleur d’application.

**Liste 5`Controllers\MoviesController.vb`**

[!code-vb[Main](passing-data-to-view-master-pages-vb/samples/sample5.vb)]

Le contrôleur de films, tout comme le contrôleur de la `Index()` maison `Details()`discuté dans la section précédente, expose deux méthodes d’action nommées et . Notez que la liste des catégories de films affichées par `Index()` la `Details()` page principale de vue n’est pas ajoutée pour afficher les données dans le ou l’autre. Étant donné que le contrôleur des films hérite du contrôleur d’application, la liste des catégories de films est ajoutée pour afficher automatiquement les données.

Notez que cette solution à l’ajout de données de vue pour une page principale de vue ne viole pas le principe DRY (Ne vous répétez pas). Le code d’ajout de la liste des catégories de films pour afficher les données n’est contenu qu’en un seul endroit : le constructeur du contrôleur d’application.

### <a name="summary"></a>Récapitulatif

Dans ce tutoriel, nous avons discuté de deux approches pour transmettre les données de vue d’un contrôleur à une page principale de vue. Tout d’abord, nous avons examiné une approche simple, mais difficile à maintenir. Dans la première section, nous avons discuté de la façon dont vous pouvez ajouter des données de vue pour une page principale de vue dans chaque action de contrôleur dans votre application. Nous avons conclu qu’il s’agissait d’une mauvaise approche parce qu’elle viole le principe DRY (Don’t Repeat Yourself).

Ensuite, nous avons examiné une bien meilleure stratégie pour ajouter les données requises par une page principale de visualisation pour afficher les données. Au lieu d’ajouter les données de vue dans chaque action de contrôleur, nous n’avons ajouté les données de vue qu’une seule fois dans un contrôleur d’application. De cette façon, vous pouvez éviter le code en double lors de la transmission des données à une page principale de vue dans une application ASP.NET MVC.

> [!div class="step-by-step"]
> [Précédent](creating-page-layouts-with-view-master-pages-vb.md)
