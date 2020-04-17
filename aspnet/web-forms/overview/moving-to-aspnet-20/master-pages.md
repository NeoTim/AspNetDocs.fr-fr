---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Pages Maîtresses Microsoft Docs
author: rick-anderson
description: L’un des éléments clés d’un site Web réussi est un look et une sensation constants. Dans ASP.NET 1.x, les développeurs ont utilisé des contrôles utilisateur pour reproduire la page elem commune...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: b493feb21d2e3d6429f0a23df5aab66e0c4c5b07
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543182"
---
# <a name="master-pages"></a>Pages maîtres

par [Microsoft](https://github.com/microsoft)

> L’un des éléments clés d’un site Web réussi est un look et une sensation constants. Dans ASP.NET 1.x, les développeurs ont utilisé des contrôles utilisateur pour reproduire des éléments de page communs sur une application Web. Bien qu’il s’agit certainement d’une solution réalisable, l’utilisation des contrôles utilisateur a quelques inconvénients. Par exemple, une modification de la position d’un contrôle utilisateur nécessite une modification de plusieurs pages sur un site. Les contrôles de l’utilisateur ne sont pas non plus rendus dans la vue de conception après avoir été insérés sur une page.

L’un des éléments clés d’un site Web réussi est un look et une sensation constants. Dans ASP.NET 1.x, les développeurs ont utilisé des contrôles utilisateur pour reproduire des éléments de page communs sur une application Web. Bien qu’il s’agit certainement d’une solution réalisable, l’utilisation des contrôles utilisateur a quelques inconvénients. Par exemple, une modification de la position d’un contrôle utilisateur nécessite une modification de plusieurs pages sur un site. Les contrôles de l’utilisateur ne sont pas non plus rendus dans la vue de conception après avoir été insérés sur une page.

ASP.NET 2.0 introduit les pages Master comme un moyen de maintenir un look et une sensation cohérents, et comme vous le verrez bientôt, les pages Master représentent une amélioration significative par rapport à la méthode de contrôle de l’utilisateur.

## <a name="why-master-pages"></a>Pourquoi Master Pages ?

Vous vous demandez peut-être pourquoi des pages maîtresses ont été nécessaires dans ASP.NET 2.0. Après tout, les développeurs de sites Web utilisent déjà les contrôles des utilisateurs dans ASP.NET 1.x pour partager des zones de contenu entre les pages. Il y a en fait plusieurs raisons pour lesquelles les contrôles utilisateur sont une solution moins que optimale pour créer une mise en page commune.

Les contrôles des utilisateurs ne définissent pas réellement la mise en page. Au lieu de cela, ils définissent la mise en page et la fonctionnalité pour une partie d’une page. La distinction entre ces deux est importante car elle rend la maniabilité d’une solution de contrôle utilisateur beaucoup plus difficile. Par exemple, lorsque vous souhaitez modifier la position d’un contrôle utilisateur sur votre page, vous devez modifier la page réelle sur laquelle le contrôle utilisateur apparaît. C’est bien si vous n’avez que quelques pages, mais dans les grands sites, il devient rapidement un cauchemar de gestion du site!

Un autre inconvénient de l’utilisation des contrôles utilisateur pour définir une mise en page commune est enracinée dans l’architecture de ASP.NET lui-même. Si un membre public d’un contrôle utilisateur est modifié, il vous oblige à recomposer toutes les pages qui utilisent le contrôle de l’utilisateur. À leur tour, ASP.NET re-JIT vos pages quand ils sont d’abord consultés. Cela, une fois de plus, produit une architecture non évolutive et un problème de gestion de site pour les grands sites.

Ces deux problèmes (et beaucoup plus) sont bien abordés par les pages maîtresses dans ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Fonctionnement des pages-maîtres

Une page maîtresse est analogue à un modèle pour d’autres pages. Les éléments de la page qui doivent être partagés entre d’autres pages (c.-à-d. menus, bordures, etc.) sont ajoutés à la page principale. Lorsque de nouvelles pages sont ajoutées au site, vous pouvez les associer à une page maîtresse. Une page associée à une page maîtresse s’appelle une **page de contenu**. Par défaut, une page de contenu prend l’apparence de la page principale. Toutefois, lorsque vous créez une page principale, vous pouvez définir des parties de la page que la page de contenu peut remplacer par son propre contenu. Ces parties sont définies à l’aide d’un nouveau contrôle introduit dans ASP.NET 2.0; le contrôle **ContentPlaceHolder.**

Une page maîtresse peut contenir n’importe quel nombre de contrôles ContentPlaceHolder (ou aucun du tout.) Sur la page de contenu, le contenu des contrôles ContentPlaceHolder apparaît à l’intérieur des contrôles de **contenu,** un autre nouveau contrôle dans ASP.NET 2.0. Par défaut, les pages de contenu contrôlent le contenu afin que vous puissiez fournir votre propre contenu. Si vous souhaitez utiliser le contenu de la page principale à l’intérieur des contrôles de contenu, vous pouvez le faire comme vous le verrez plus tard dans ce module. Le contrôle du contenu est cartographié sur le contrôle ContentPlaceHolder via l’attribut ContentPlaceHolderID du contrôle de contenu. Le code ci-dessous cartographie un contrôle de contenu à un contrôle ContentPlaceHolder appelé mainBody sur une page principale.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Vous entendrez souvent les gens décrire les pages maîtresses comme étant une classe de base pour d’autres pages. Ce n’est pas vrai. La relation entre les pages maîtresses et les pages de contenu n’est pas celle de l’héritage.

**La figure 1** affiche une page maîtresse et une page de contenu associée telles qu’elles apparaissent dans Visual Studio 2005. Vous pouvez voir le contrôle ContentPlaceHolder dans la page principale et le contrôle de contenu correspondant dans la page de contenu. Notez que le contenu des pages maîtresses qui est en dehors du ContentPlaceHolder est visible mais grisé dans la page de contenu. Seul le contenu à l’intérieur du ContentPlaceHolder peut être supplanté par la page de contenu. Tous les autres contenus qui proviennent de la page principale sont immuables.

![Une page maîtresse et sa page de contenu associée](master-pages/_static/image1.jpg)

**Figure 1**: Une page maîtresse et sa page de contenu associée

## <a name="creating-a-master-page"></a>Création d’une page Maîtresse

Pour créer une nouvelle page maîtresse :

1. Ouvrez Visual Studio 2005 et créez un nouveau site Web.
2. Cliquez sur Fichier, Nouveau, Fichier.
3. Choisissez Master File parmi le dialogue Add New Item tel qu’indiqué dans **la figure 2**.
4. Cliquez sur Ajouter.

![Création d’une nouvelle page Master](master-pages/_static/image2.jpg)

**Figure 2**: Création d’une nouvelle page Master

Notez que l’extension du fichier pour une page principale est *.master*. C’est l’une des façons dont une page maîtresse diffère d’une page ordinaire. L’autre différence principale est @Page qu’au lieu d’une directive, la page principale contient une @Master directive. Passez à Source View pour la page principale que vous venez de créer et examinez le code.

Une nouvelle page principale aura un contrôle ContentPlaceHolder par défaut. Dans la plupart des cas, il est plus logique de créer les éléments de page communs d’abord, puis d’insérer les contrôles ContentPlaceHolder où le contenu personnalisé est souhaité. Dans ces cas, les développeurs voudront supprimer le contrôle par défaut contentPlaceHolder et en insérer de nouveaux au fur et à mesure que la page sera développée. Les contrôles ContentPlaceHolder ne sont pas resizables malgré le fait qu’ils affichent des poignées de dimensionnement. Les tailles de contrôle ContentPlaceHolder sont automatiquement basées sur le contenu qu’il contient à une exception près ; si vous placez un contrôle ContentPlaceHolder à l’intérieur d’un élément de bloc tel qu’une cellule de table, il taillera en fonction de la taille de l’élément.

## <a name="lab-1-working-with-master-pages"></a>Laboratoire 1 Travailler avec les Pages Master

Dans ce laboratoire, vous créerez une nouvelle page maître et définirez trois contrôles ContentPlaceHolder. Vous créerez ensuite une nouvelle page de contenu et remplacerez le contenu à partir d’au moins un des contrôles ContentPlaceHolder.

1. Créez une page maîtresse et insérez les contrôles ContentPlaceHolder. 

    1. Créez une nouvelle page maîtresse comme décrit ci-dessus.
    2. Supprimer le contrôle par défaut ContentPlaceHolder.
    3. Sélectionnez le contrôle ContentPlaceHolder en cliquant sur la bordure supérieure ombragée du contrôle, puis supprimez-le en frappant la touche DEL sur votre clavier.
    4. Insérez une nouvelle table à l’aide de *l’en-tête et* du modèle latéral comme indiqué dans la figure 3. Changez la largeur et la hauteur à 90% chacun de sorte que toute la table est visible dans le concepteur.

![](master-pages/_static/image3.jpg)

**Figure 3**

1. Placez le curseur dans chaque cellule de la table et placez la propriété *valign* *au dessus.*
2. De la boîte à outils, insérez un contrôle ContentPlaceHolder dans la cellule supérieure de la table (la cellule d’en-tête.)
3. Lorsque vous insérez ce contrôle ContentPlaceHolder, vous remarquerez que la hauteur de la ligne prendra presque toute la page comme indiqué dans la figure 4. Ne vous inquiétez pas à ce sujet à ce stade.

![L’espace vide est dans la même cellule que le ContentPlaceHolder](master-pages/_static/image1.gif)

**Figure 4**: L’espace vide est dans la même cellule que le ContentPlaceHolder

1. Placez un contrôle ContentPlaceHolder dans les deux autres cellules. Une fois que les autres contrôles ContentPlaceHolder ont été insérés, la taille des cellules de table devrait être comme vous vous y attendriez. La page doit maintenant ressembler à la page indiquée dans **la figure 5**.

![Le Master avec tous les contrôles ContentPlaceHolder. Notez que la hauteur de la cellule pour la cellule d’en-tête est maintenant ce qu’elle devrait être](master-pages/_static/image2.gif)

**Figure 5**: Le Maître avec tous les contrôles ContentPlaceHolder. Notez que la hauteur de la cellule pour la cellule d’en-tête est maintenant ce qu’elle devrait être

1. Entrez un texte de votre choix dans chacun des trois contrôles ContentPlaceHolder.
2. Enregistrer la page principale comme exercise1.master.
3. Créez un nouveau formulaire Web et associez-le à la page maître exercise1.master.
4. Sélectionnez Fichier, Nouveau, Fichier dans Visual Studio 2005.
5. Sélectionnez **formulaire Web** dans le dialogue Ajouter un nouvel élément.
6. Assurez-vous que la case à cocher de la page principale Select est vérifiée comme indiqué dans la figure 6.

![Ajout d’une nouvelle page de contenu](master-pages/_static/image3.gif)

**Figure 6**: Ajout d’une nouvelle page de contenu

1. Cliquez sur Ajouter.
2. Sélectionnez exercise1.master dans le Sélectionnez un dialogue de page principale tel qu’indiqué dans la figure 7.
3. Cliquez sur OK pour ajouter la nouvelle page de contenu.

La nouvelle page de contenu apparaît dans Visual Studio avec un contrôle de contenu pour chaque contrôle ContentPlaceHolder sur la page principale. Par défaut, les contrôles de contenu sont vides afin que vous puissiez ajouter votre propre contenu. Si vous souhaitez qu’ils utilisent le contenu du contrôle ContentPlaceHolder sur la page principale, il suffit de cliquer sur le symbole de l’étiquette intelligente (la petite flèche noire dans le coin supérieur droit du contrôle) et choisissez *par défaut de contenu de maîtres à* partir de l’étiquette intelligente comme indiqué dans la figure **8**. Lorsque vous le faites, l’élément du menu change pour *créer du contenu personnalisé*. En cliquant à ce moment-là, supprime le contenu de la page principale vous permettant de définir du contenu personnalisé pour ce contrôle de contenu particulier.

![Définir un contrôle de contenu par défaut au contenu des pages Master](master-pages/_static/image4.gif)

**Figure 7**: Définir un contrôle de contenu par défaut au contenu des pages maîtresses

## <a name="connecting-master-page-and-content-pages"></a>Connexion des pages Master Page et Contenu

L’association entre une page maîtresse et une page de contenu peut être configurée de l’une des quatre façons différentes :

- <strong>L’attribut MasterPageFile</strong> de la @Page directive
- Définir la propriété **Page.MasterPageFile** en code.
- ** &lt;L’élément pages&gt; ** dans le fichier de configuration des applications (web.config dans le dossier racine de l’application)
- ** &lt;L’élément pages&gt; ** dans un fichier de configuration de sous-plieurs (web.config dans un sous-pli)

## <a name="masterpagefile-attribute"></a>Attribut MasterPageFile

L’attribut MasterPageFile facilite l’application d’une page maîtresse à une page ASP.NET particulière. C’est aussi la méthode utilisée pour appliquer la page principale lorsque vous vérifiez la case à cocher **Select Master Page** comme vous l’avez fait dans l’exercice 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Définir Page.MasterPageFile dans le Code

En définissant la propriété MasterPageFile en code, vous pouvez appliquer une page maîtresse particulière à votre contenu à l’heure d’exécution. Ceci est utile dans les cas où vous devrez peut-être appliquer une page principale spécifique basée sur un rôle d’utilisateur ou d’autres critères. La propriété MasterPageFile doit être définie dans la méthode PreInit. S’il est réglé après la méthode PreInit, une modification invalide sera lancée. La page sur laquelle cette propriété est définie doit également avoir un contrôle de contenu comme le contrôle de haut niveau pour la page. Sinon, une impression httpexception sera lancée lorsque la propriété MasterPageFile sera définie.

## <a name="using-the-ltpagesgt-element"></a>Utilisation &lt;des&gt; pages Element

Vous pouvez configurer une page maîtresse pour vos pages &lt;en&gt; définissant l’attribut masterPageFile dans l’élément pages du fichier web.config. Lors de l’utilisation de cette méthode, gardez à l’esprit que les fichiers web.config plus bas dans la structure d’application peuvent remplacer ce paramètre. Tout attribut MasterPageFile @Page défini dans une directive remplacera également ce paramètre. L’utilisation&gt; de l’élément pages facilite la &lt;création d’une page maître *qui* peut être remplacée si nécessaire dans des dossiers ou des fichiers particuliers.

## <a name="properties-in-master-pages"></a>Propriétés dans les pages maîtresses

Une page maîtresse peut exposer les propriétés en rendant simplement ces propriétés publiques dans la page principale. Par exemple, le code suivant définit une propriété appelée SomeProperty :

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Pour accéder à la propriété SomeProperty à partir de la page Contenu, vous devrez utiliser la propriété Master comme il le souhaitez :

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Pages Maîtresses de nidification

Les pages maîtresses sont la solution parfaite pour assurer un look et une sensation communs à travers une grande application Web. Cependant, il n’est pas rare que certaines parties d’un grand site partagent une interface commune tandis que d’autres parties partagent une interface différente. Pour répondre à ce besoin, plusieurs pages maîtresses sont la solution parfaite. Toutefois, cela ne tient toujours pas compte du fait qu’une grande application peut avoir certains composants (comme un menu, par exemple) qui sont partagés entre toutes les pages et autres composants qui ne sont partagés qu’entre certaines sections du site. Pour ce type de situation, les pages maîtresses imbriquées remplissent bien le besoin. Comme vous l’avez vu, une page principale normale se compose d’une page maîtresse et d’une page de contenu. Dans une situation de page principale imbriquée, il y a deux pages maîtresses ; un maître parent et un maître d’enfant. La page maître enfant est également une page de contenu et son maître est la page principale parent.

Voici le code d’une page maîtresse typique :

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

Dans un scénario de maître imbriqué, ce serait le maître parent. Une autre page principale utiliserait cette page comme sa page principale, et ce code ressemblerait à ceci :

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Notez que dans ce scénario, le maître de l’enfant est également une page de contenu pour le maître parent. Tout le contenu du maître de l’enfant apparaît à l’intérieur d’un contrôle de contenu qui tire son contenu du contrôle contentPlaceHolder du parent.

> [!NOTE]
> Le support du concepteur n’est pas disponible pour les pages maîtresses imbriquées. Lorsque vous développez à l’aide de maîtres imbriqués, vous devrez utiliser la vue source.

Cette vidéo montre une procédure pas à pas d’utilisation de pages maîtresses imbriquées.

![](master-pages/_static/image1.png)

[Ouvrez la vidéo plein écran](master-pages/_static/nested1.wmv)

![Sélection d’une page Master](master-pages/_static/image4.jpg)

**Figure 8**: Sélection d’une page maîtresse
