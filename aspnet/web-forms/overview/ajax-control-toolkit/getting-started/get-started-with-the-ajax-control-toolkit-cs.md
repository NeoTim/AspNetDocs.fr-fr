---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Démarrer avec la boîte à outils de contrôle AJAX (C) Microsoft Docs
author: rick-anderson
description: Apprenez tout ce que vous devez savoir pour commencer à utiliser la boîte à outils de contrôle AJAX.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: b5954019ec3312f06f38012e4d3f9b1f71573f76
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543585"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a>Bien démarrer avec AJAX Control Toolkit (C#)

par [Microsoft](https://github.com/microsoft)

> Apprenez tout ce que vous devez savoir pour commencer à utiliser la boîte à outils de contrôle AJAX.

La boîte à outils de contrôle AJAX contient plus de 30 contrôles gratuits que vous pouvez utiliser dans vos applications ASP.NET. Dans ce tutoriel, vous apprenez à télécharger la boîte à outils de contrôle AJAX et ajouter les commandes de la boîte à outils à votre visual Studio/Visual Web Developer Express toolbox.

## <a name="downloading-the-ajax-control-toolkit"></a>Téléchargement de la boîte à outils de contrôle AJAX

La [boîte à outils de contrôle AJAX](http://devexpress.com/act) est un projet open source développé par les membres de la communauté ASP.NET et l’équipe ASP.NET. 

[![Téléchargement de la boîte à outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Figure 01**: Télécharger la boîte à outils de contrôle AJAX[(Cliquez pour voir l’image grandeur nature)](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png)

Après avoir téléchargé le fichier, vous devez débloquer le fichier. Cliquez à droite sur le fichier, sélectionnez les propriétés et cliquez sur le bouton **Déblocage** (voir la figure 2).

[![Déblocage du fichier ZIP AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Figure 02**: Déblocage du fichier ZIP AJAX Control Toolkit ([Cliquez pour voir l’image grandeur nature](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))

Après avoir débloqué le fichier, vous pouvez décompresser le fichier : cliquez à droite sur le fichier et sélectionner **l’option Extrait Tout** menu. Maintenant, nous sommes prêts à ajouter la boîte à outils à la boîte à outils Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Ajout de la boîte à outils de contrôle AJAX à la boîte à outils

La façon la plus simple d’utiliser la boîte à outils de contrôle AJAX est d’ajouter la boîte à outils à votre boîte à outils Visual Studio/Visual Web Developer (voir la figure 3). De cette façon, vous pouvez simplement faire glisser un contrôle de boîte à outils sur une page lorsque vous voulez l’utiliser.

[![La boîte à outils de contrôle AJAX apparaît dans la boîte à outils](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Figure 03**: La boîte à outils DE contrôle AJAX apparaît dans la boîte à outils[(Cliquez pour voir l’image grandeur nature](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))

Tout d’abord, vous devez ajouter un onglet AJAX Control Toolkit à la boîte à outils. Effectuez les opérations suivantes.

1. Créez un nouveau site Web ASP.NET en sélectionnant l’option de menu Fichier, Nouveau site Web. Double-cliquez sur le Default.aspx dans la fenêtre Solution Explorer pour ouvrir le fichier dans l’éditeur.
2. Cliquez à droite sur la boîte à outils sous l’onglet général et sélectionnez l’option de menu **Ajouter l’onglet** (voir la figure 4).
3. Entrez un nouvel onglet nommé AJAX Control Toolkit.

[![Ajout d’un nouvel onglet](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Figure 04**: Ajout d’un nouvel onglet[(Cliquez pour voir l’image grandeur nature)](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png)

Ensuite, vous devez ajouter les commandes ajax Control Toolkit à la nouvelle onglet. Suivez ces étapes :

- Cliquez à droite sous l’onglet AJAX Control Toolkit et sélectionnez l’option **menu Choisissez des éléments (voir figure 5)**.
- Naviguez à l’endroit où vous avez décompressé la boîte à outils de contrôle AJAX et sélectionnez l’assemblage AjaxControlToolkit.dll.

[![Choisissez des éléments à ajouter à la boîte à outils](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Figure 05**: Choisissez des éléments à ajouter à la boîte à outils[(Cliquez pour voir l’image grandeur nature](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))

Une fois que vous avez terminé ces étapes, tous les contrôles de boîte à outils apparaîtront dans votre boîte à outils.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Mise à niveau vers une nouvelle version de la boîte à outils

Si vous utilisiez une version plus ancienne de la boîte à outils et maintenant besoin de passer à une version ultérieure voici les étapes recommandées:

- Binaires - Supprimez l’ancienne version de l’assemblage AjaxControlToolkit.dll à partir de votre dossier Bin site.
- Articles de boîte à outils - Supprimer l’onglet AJAX Control Toolkit et suivre les étapes ci-dessus pour recréer l’onglet avec la nouvelle version de l’assemblage AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Suivant](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
