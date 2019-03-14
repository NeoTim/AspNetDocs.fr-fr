---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: ContentPlaceHolders multiples et contenu par défaut (c#) | Microsoft Docs
author: rick-anderson
description: Examine comment ajouter plusieurs espaces réservés contenu à une page maître, ainsi que comment spécifier le contenu par défaut dans les espaces réservés contenu.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: 86b01fd7ccf08c656c8d8dc5f17be01fcd463fca
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57035596"
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>ContentPlaceHolders multiples et contenu par défaut (C#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Examine comment ajouter plusieurs espaces réservés contenu à une page maître, ainsi que comment spécifier le contenu par défaut dans les espaces réservés contenu.


## <a name="introduction"></a>Introduction

Dans le didacticiel précédent, nous avons examiné comment activer des pages maîtres pour créer une disposition cohérente de l’échelle du site, les développeurs ASP.NET. Pages maîtres définissent le balisage qui est commun à toutes ses pages de contenu et les régions qui sont personnalisables sur une base de page par page. Dans le didacticiel précédent, nous avons créé une page maître simple (`Site.master`) et deux pages de contenu (`Default.aspx` et `About.aspx`). Notre page maître est composé de deux ContentPlaceHolders nommés `head` et `MainContent`, qui se trouvaient dans le `<head>` élément et le Web Form, respectivement. Tandis que les pages de contenu chaque avaient deux contrôles de contenu, nous avons spécifié uniquement le balisage pour celui correspondant à `MainContent`.

Comme le démontre les deux contrôles ContentPlaceHolder dans `Site.master`, une page maître peut contenir plusieurs ContentPlaceHolders. De plus, la page maître peut spécifier un balisage par défaut pour les contrôles ContentPlaceHolder. Une page de contenu, puis, peut éventuellement spécifier son propre balisage ou utiliser le balisage par défaut. Dans ce didacticiel, nous examinons d’à l’aide de plusieurs contrôles de contenu dans la page maître et voir comment définir le balisage par défaut dans les contrôles ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Étape 1 : Ajout de contrôles ContentPlaceHolder supplémentaires à la Page maître

De nombreuses conceptions de site Web contient plusieurs domaines personnalisés sur une base de page par page à l’écran. `Site.master`, la page maître, nous avons créé dans le didacticiel précédent, contient un ContentPlaceHolder unique dans le formulaire Web nommé `MainContent`. Plus précisément, cette ContentPlaceHolder se trouve dans le `mainContent` `<div>` élément.

La figure 1 montre `Default.aspx` lorsqu’ils sont affichés via un navigateur. La zone entourée en rouge est le balisage spécifiques à la page correspondant à `MainContent`.


[![La région encerclée affiche la zone personnalisable actuellement sur une base de Page par Page](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Figure 01**: La région avec cercle affiche la zone actuellement personnalisable sur une base de Page par Page ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))


Imaginez qu’en plus de la région indiquée dans la Figure 1, nous devons également ajouter des éléments spécifiques à la page à la colonne de gauche sous les leçons et les actualités sections. Pour ce faire, nous ajoutons un autre contrôle ContentPlaceHolder à la page maître. Pour suivre la procédure, ouvrez le `Site.master` page dans Visual Web Developer maître et faites glisser un contrôle ContentPlaceHolder à partir de la boîte à outils vers le concepteur, après la section d’actualités. Définir le ContentPlaceHolder `ID` à `LeftColumnContent`.


[![Ajouter un contrôle ContentPlaceHolder à la colonne de gauche de la Page maître](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Figure 02**: Ajouter un contrôle ContentPlaceHolder à la colonne de gauche de la Page maître ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))


Avec l’ajout de la `LeftColumnContent` ContentPlaceHolder à la page maître, nous pouvons définir le contenu de cette région sur une base de page par page en incluant un contenu contrôle dans la page dont `ContentPlaceHolderID` est défini sur `LeftColumnContent`. Nous examinons ce processus à l’étape 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Étape 2 : Définition du contenu pour le nouveau ContentPlaceHolder dans les Pages de contenu

Lorsque vous ajoutez une nouvelle page de contenu au site Web, Visual Web Developer crée automatiquement un contenu de contrôle dans la page pour chaque ContentPlaceHolder dans la page maître sélectionnée. Après avoir ajouté un le `LeftColumnContent` ContentPlaceHolder à notre page maître à l’étape 1, nouvelle sera de pages ASP.NET désormais avoir trois contrôles de contenu.

Pour illustrer cela, ajoutez une nouvelle page de contenu dans le répertoire racine nommé `MultipleContentPlaceHolders.aspx` qui est lié à la `Site.master` page maître. Visual Web Developer crée cette page avec le balisage déclaratif suivant :

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Entrez du contenu dans le contrôle de contenu faisant référence à la `MainContent` ContentPlaceHolders (`Content2`). Ensuite, ajoutez le balisage suivant à la `Content3` contrôle de contenu (qui référence le `LeftColumnContent` ContentPlaceHolder) :

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Après avoir ajouté ce balisage, visitez la page via un navigateur. Comme le montre la Figure 3, le balisage est placé dans le `Content3` contrôle de contenu s’affiche dans la colonne de gauche sous la section Actualités (entourée en rouge). Le balisage est placé dans `Content2` s’affiche dans la partie droite de la page (encerclée en bleu).


[![La colonne de gauche inclut désormais le contenu propre à la Page sous la Section Actualités](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Figure 03**: La colonne maintenant inclut spécifiques à la Page contenu sous le News Section gauche ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Définition du contenu dans les Pages de contenu existants

Création d’une nouvelle page de contenu automatiquement incorpore le contrôle ContentPlaceHolder que nous avons ajouté à l’étape 1. Mais nos deux pages de contenu existants - `About.aspx` et `Default.aspx` -ne pas avoir un contrôle de contenu pour le `LeftColumnContent` ContentPlaceHolder. Pour spécifier le contenu de cette ContentPlaceHolder dans ces deux pages existantes, nous devons ajouter un contrôle de contenu nous-mêmes.

Contrairement à la plupart des contrôles Web ASP.NET, la boîte à outils de Visual Web Developer n’inclut pas un élément de contrôle de contenu. Nous pouvons taper manuellement dans le balisage déclaratif du contrôle contenu dans la vue de Source, mais une approche plus facile et plus rapide consiste à utiliser le mode Création. Ouvrez le `About.aspx` page et basculer vers la vue de conception. Comme le montre la Figure 4, le `LeftColumnContent` ContentPlaceHolder s’affiche en mode Design ; si vous déplacez la souris dessus, le titre affiché lit : « LeftColumnContent (Master) ». L’inclusion de « Master » dans le titre indique qu’il n’existe aucun contrôle de contenu défini dans la page pour cette ContentPlaceHolder. S’il existe un contrôle de contenu de ContentPlaceHolder, comme dans le cas pour `MainContent`, lit le titre : «*ContentPlaceHolderID* (personnalisé). »

Pour ajouter un contrôle de contenu pour le `LeftColumnContent` ContentPlaceHolder à `About.aspx`, développez la balise active de ContentPlaceHolder, puis cliquez sur le lien Créer un contenu personnalisé.


[![La vue de conception de About.aspx affiche LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Figure 04**: La vue de conception pour `About.aspx` montre le `LeftColumnContent` ContentPlaceHolder ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))


Cliquant sur le lien Créer un contenu personnalisé génère nécessaires contrôle dans la page et les ensembles de contenu son `ContentPlaceHolderID` propriété le ContentPlaceHolder `ID`. Par exemple, en cliquant sur le lien Créer un contenu personnalisé pour `LeftColumnContent` région dans `About.aspx` ajoute le balisage déclaratif suivant à la page :

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>L’omission de contrôles de contenu

ASP.NET ne nécessite pas que toutes les pages de contenu incluent des contrôles de contenu pour chaque ContentPlaceHolder défini dans la page maître. Si un contrôle de contenu est omis, le moteur ASP.NET utilise le balisage défini dans ContentPlaceHolder dans la page maître. Ce balisage est appelé le ContentPlaceHolder *contenu par défaut* et est utile dans les scénarios où le contenu pour une région est commune à la plupart des pages, mais doit être personnalisé pour un petit nombre de pages. Étape 3 explore en spécifiant contenu par défaut dans la page maître.

Actuellement, `Default.aspx` contient deux contrôles de contenu pour le `head` et `MainContent` ContentPlaceHolders ; il n’a pas d’un contrôle de contenu pour `LeftColumnContent`. Par conséquent, lorsque `Default.aspx` est rendu le `LeftColumnContent` le contenu de ContentPlaceHolder par défaut est utilisé. Étant donné que nous devons encore définir n’importe quel contenu par défaut pour cette ContentPlaceHolder, l’effet net est qu’aucune balise n’est émis pour cette région. Pour vérifier ce comportement, visitez `Default.aspx` via un navigateur. Comme le montre la Figure 5, aucune balise n’est émis dans la colonne de gauche sous la section d’actualités.


[![Aucun contenu n’est rendu pour LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Figure 05**: Aucun contenu n’est rendu pour le `LeftColumnContent` ContentPlaceHolder ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Étape 3 : Spécification du contenu par défaut dans la Page maître

Certaines conceptions de site Web incluent une région dont le contenu est identique pour toutes les pages du site à l’exception d’une ou deux exceptions. Envisagez d’un site Web qui prend en charge les comptes d’utilisateur. Un tel site nécessite une page de connexion où les visiteurs peuvent saisir leurs informations d’identification pour vous connecter au site. Pour accélérer le processus de connexion, les concepteurs de sites Web peuvent inclure des zones de texte Nom d’utilisateur et mot de passe dans le coin supérieur gauche de chaque page pour autoriser les utilisateurs à se connecter sans avoir à visiter explicitement la page de connexion. Si ces zones de texte Nom d’utilisateur et mot de passe sont utiles dans la plupart des pages, elles sont obsolètes dans la page de connexion, qui contient déjà des zones de texte pour les informations d’identification de l’utilisateur.

Pour implémenter cette conception, vous pouvez créer un contrôle ContentPlaceHolder dans le coin supérieur gauche de la page maître. Chaque page que nécessaire pour afficher les zones de texte Nom d’utilisateur et mot de passe dans le coin supérieur gauche voulez-vous créer un contrôle de contenu pour ce ContentPlaceHolder et ajouter l’interface nécessaire. La page de connexion, en revanche, serait omettre soit l’ajout d’un contrôle de contenu pour ce ContentPlaceHolder ou créerait un contenu contrôle avec aucune balise définie. L’inconvénient de cette approche est que nous devons n’oubliez pas d’ajouter les zones de texte Nom d’utilisateur et mot de passe à chaque page que nous ajoutons au site (à l’exception de la page de connexion). Cela peut entraîner des problèmes. Nous sommes susceptibles d’oublier d’ajouter ces zones de texte à une page ou deux ou, pire encore, nous ne pouvons pas implémenter l’interface correctement (peut-être en ajoutant simplement une zone de texte au lieu de deux).

Une meilleure solution consiste à définir les zones de texte Nom d’utilisateur et mot de passe en tant que contenu de valeur par défaut de ContentPlaceHolder. En procédant ainsi, il nous suffit de remplacer ce contenu par défaut dans les quelques pages qui ne s’affichent pas les zones de texte Nom d’utilisateur et mot de passe (la page de connexion, par exemple). Pour illustrer la spécification de contenu par défaut pour un contrôle ContentPlaceHolder, nous allons implémenter le scénario décrit seulement.

> [!NOTE]
> Le reste de ce didacticiel met à jour notre site Web pour inclure une interface de connexion dans la colonne de gauche pour toutes les pages, mais la page de connexion. Toutefois, ce didacticiel n’examine pas comment configurer le site Web pour prendre en charge les comptes d’utilisateur. Pour plus d’informations sur ce sujet, reportez-vous à mon [l’authentification par formulaire, autorisation, des comptes d’utilisateur et les rôles](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) didacticiels.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Ajout d’un ContentPlaceHolder et en spécifiant son contenu par défaut

Ouvrez le `Site.master` page maître et ajoutez le balisage suivant à la colonne gauche entre la `DateDisplay` section étiquette et les leçons :

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Après avoir ajouté ce balisage mode de création de votre page maître doit ressembler à la Figure 6.


[![La Page maître inclut un contrôle de connexion](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Figure 06**: La Page maître inclut un contrôle de connexion ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))


Cette ContentPlaceHolder, `QuickLoginUI`, a un contrôle Web de connexion en tant que son contenu par défaut. Le contrôle de connexion affiche une interface utilisateur qui invite l’utilisateur à leur nom d’utilisateur et le mot de passe, ainsi que d’un bouton de connexion. Lorsque vous cliquez sur le bouton se connecter, le contrôle de connexion valide en interne les informations d’identification utilisateur par rapport à l’API d’appartenance. Ensuite, pour utiliser ce contrôle de connexion dans la pratique, vous devez configurer votre site pour utiliser l’appartenance. Cette rubrique est dépasse le cadre de ce didacticiel. reportez-vous à mon [l’authentification par formulaire, autorisation, des comptes d’utilisateur et les rôles](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) didacticiels pour plus d’informations sur la création d’une application web qui prend en charge les comptes d’utilisateur.

N’hésitez pas à personnaliser le comportement ou l’apparence du contrôle de connexion. J’ai défini deux de ses propriétés : `TitleText` et `FailureAction`. Le `TitleText` valeur de la propriété valeur par défaut est « Log In », s’affiche en haut de l’interface du contrôle utilisateur. J’ai défini cette propriété afin qu’il affiche le texte « De « connexion en tant qu’un `<h3>` élément. Le `FailureAction` propriété indique que faire si les informations d’identification ne sont pas valides. Les valeurs par défaut à une valeur de `Refresh`, ce qui laisse l’utilisateur sur la même page et affiche un message d’échec dans le contrôle de connexion. J’ai changé à `RedirectToLoginPage`, qui envoie l’utilisateur vers la page de connexion en cas d’informations d’identification non valides. Je préfère envoyer l’utilisateur vers la page de connexion lorsqu’un utilisateur tente de se connecter à partir d’une autre page, mais échoue, étant donné que la page de connexion peut contenir des instructions supplémentaires et des options qui ne tiendraient pas facilement dans la colonne de gauche. Par exemple, la page de connexion peut inclure des options pour récupérer un mot de passe oublié ou pour créer un nouveau compte.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Création de la Page de connexion et en remplaçant le contenu par défaut

Avec la page maître terminée, notre étape suivante consiste à créer la page de connexion. Ajouter une page ASP.NET pour le répertoire de racine de votre site nommé `Login.aspx`, liant à le `Site.master` page maître. Ceci créera une page avec quatre contrôles de contenu, un pour chacune des ContentPlaceHolders défini dans `Site.master`.

Ajouter un contrôle de connexion à la `MainContent` contrôle de contenu. De même, n’hésitez pas à ajouter du contenu à la `LeftColumnContent` région. Toutefois, veillez à laisser le contrôle de contenu pour le `QuickLoginUI` ContentPlaceHolder vide. Cela garantit que la connexion de contrôle n’apparaît pas dans la colonne gauche de la page de connexion.

Après avoir défini le contenu pour le `MainContent` et `LeftColumnContent` régions, balisage déclaratif de votre page de connexion doit ressembler à ce qui suit :

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

Figure 7 illustre cette page lorsqu’ils sont affichés via un navigateur. Étant donné que cette page spécifie un contrôle de contenu pour le `QuickLoginUI` ContentPlaceHolder, ce paramètre remplace le contenu par défaut spécifié dans la page maître. L’effet net est que le contrôle de connexion affiché dans la conception de la page maître (voir Figure 6) n'est pas restituée dans cette page.


[![La Page de connexion Represses contenu de par défaut de la QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Figure 07**: La Page de connexion Represses la `QuickLoginUI` par défaut contenu de ContentPlaceHolder ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>À l’aide du contenu par défaut dans les nouvelles Pages

Nous voulons afficher le contrôle de connexion dans la colonne de gauche pour toutes les pages à l’exception de la page de connexion. Pour ce faire, toutes les pages de contenu à l’exception de la page de connexion doivent omettre un contrôle de contenu pour le `QuickLoginUI` ContentPlaceHolder. En omettant un contrôle de contenu, contenu par défaut de ContentPlaceHolder sera utilisé à la place.

Nos pages de contenu existants - `Default.aspx`, `About.aspx`, et `MultipleContentPlaceHolders.aspx` -n’incluent pas d’un contrôle de contenu pour `QuickLoginUI` , car ils ont été créés avant d’ajouter ce contrôle ContentPlaceHolder à la page maître. Par conséquent, ces pages existantes n’avez pas besoin de mettre à jour. Toutefois, les nouvelles pages ajoutées au site Web incluent un contrôle de contenu pour le `QuickLoginUI` ContentPlaceHolder, par défaut. Par conséquent, nous devons n’oubliez pas de supprimer ces contrôles de contenu chaque fois que nous ajoutons une nouvelle page de contenu (à moins que nous souhaitons remplacer le contenu de valeur par défaut de ContentPlaceHolder, comme dans le cas de la page de connexion).

Pour supprimer le contrôle de contenu, vous pouvez supprimer son balisage déclaratif à partir de la vue de Source soit manuellement, à partir de la vue conception, choisissez la valeur par défaut pour le lien de contenu de la forme de base à partir de sa balise active. Chacune de ces approches supprime le contrôle de contenu à partir de la page, puis produit les mêmes net effet.

La figure 8 illustre `Default.aspx` lorsqu’ils sont affichés via un navigateur. N’oubliez pas que `Default.aspx` a uniquement deux contrôles de contenu spécifiées dans son balisage déclaratif - un pour `head` et l’autre pour `MainContent`. Par conséquent, la valeur par défaut du contenu pour le `LeftColumnContent` et `QuickLoginUI` ContentPlaceHolders sont affichés.


[![Le contenu par défaut pour les LeftColumnContent QuickLoginUI ContentPlaceHolders sont affichés](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Figure 08**: Le contenu par défaut pour le `LeftColumnContent` et `QuickLoginUI` ContentPlaceHolders sont affichés ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))


## <a name="summary"></a>Récapitulatif

Le modèle de page maître ASP.NET autorise un nombre arbitraire de ContentPlaceHolders dans la page maître. Quel est le plus, ContentPlaceHolders inclure du contenu par défaut, qui est émis dans le cas qu’il n’existe aucune correspondant contrôle dans la page de contenu de contenu. Dans ce didacticiel, nous avons vu comment inclure des contrôles ContentPlaceHolder supplémentaires dans la page maître et comment définir des contrôles de contenu pour ces nouvelles ContentPlaceHolders dans les pages ASP.NET nouveaux et existants. Nous avions également étudié spécifiant par défaut contenues dans un ContentPlaceHolder, ce qui est utile dans les scénarios où seule une minorité de pages doit personnaliser le sinon standardisé contenu au sein d’une région donnée.

Dans le didacticiel suivant, nous allons examiner la `head` ContentPlaceHolder dans plus de détails, voir comment définir de façon déclarative et par programme le titre, les balises meta et les autres en-têtes HTML sur une base de page par page.

Bonne programmation !

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs livres de sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier livre s’intitule [ *Sams Teach vous-même ASP.NET 3.5 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott peut être atteint à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Suchi Banerjee. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](creating-a-site-wide-layout-using-master-pages-cs.md)
> [Suivant](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
