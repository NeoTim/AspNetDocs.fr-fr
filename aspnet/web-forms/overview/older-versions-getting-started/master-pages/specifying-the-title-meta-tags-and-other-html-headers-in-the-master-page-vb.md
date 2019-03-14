---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: En spécifiant le titre, les balises Meta et les autres en-têtes HTML dans la Page maître (VB) | Microsoft Docs
author: rick-anderson
description: Examine les différentes techniques permettant de définir assortis &lt;head&gt; éléments dans la Page maître depuis la page de contenu.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 1b28e6df0e0ab25e8292b6523c9ad7482301a511
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047256"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Spécification du titre, des balises META et d’autres en-têtes HTML dans la page maître (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) ou [télécharger le PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Examine les différentes techniques permettant de définir assortis &lt;head&gt; éléments dans la Page maître depuis la page de contenu.


## <a name="introduction"></a>Introduction

Nouvelles pages maîtres créés dans Visual Studio 2008 ont, par défaut, les deux contrôles ContentPlaceHolder : l’autre nommé `head`et situé dans le `<head>` ; et un élément nommé `ContentPlaceHolder1`, placé dans un formulaire Web. L’objectif de `ContentPlaceHolder1` consiste à définir une région dans le formulaire Web qui peut être personnalisé sur une base de page par page. Le `head` ContentPlaceHolder permet aux pages à ajouter du contenu personnalisé à la `<head>` section. (Bien sûr, ces deux ContentPlaceHolders peuvent être modifiés ou supprimés, et ContentPlaceHolder supplémentaire peut-être être ajouté à la page maître. Notre page maître, `Site.master`, a actuellement quatre contrôles ContentPlaceHolder.)

Le code HTML `<head>` l’élément est utilisé en tant que référentiel pour plus d’informations sur le document de page web qui ne fait pas partie du document lui-même. Cela inclut des informations telles que titre de la page web, meta-informations utilisées par les moteurs de recherche ou de robots d’indexation internes et des liens vers des ressources externes, telles que les flux RSS, JavaScript et CSS fichiers. Il se peut que certaines de ces informations peuvent être pertinentes à toutes les pages dans le site Web. Par exemple, vous souhaiterez peut-être importer globalement les mêmes règles CSS et JavaScript fichiers pour chaque page ASP.NET. Toutefois, il existe des parties de la `<head>` élément qui sont spécifiques à la page. Le titre de page est un parfait exemple.

Dans ce didacticiel, nous examinons comment définir globaux et spécifiques à la page `<head>` balisage section dans la page maître et dans ses pages de contenu.

## <a name="examining-the-master-pagesheadsection"></a>Examen de la Page maître`<head>`Section

Le fichier de page maître par défaut créé par Visual Studio 2008 contient le balisage suivant dans son `<head>` section :


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Notez que le `<head>` élément contient un `runat="server"` attribut, qui indique qu’il s’agit d’un contrôle serveur (plutôt que du HTML statique). Toutes les pages ASP.NET dérivent le [ `Page` classe](https://msdn.microsoft.com/library/system.web.ui.page.aspx), qui se trouve dans le `System.Web.UI` espace de noms. Cette classe contient un [ `Header` propriété](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) qui fournit l’accès à la page `<head>` région. À l’aide de la `Header` propriété nous pouvons définir le titre d’une page ASP.NET ou ajouter des balises supplémentaires pour le rendu `<head>` section. Il est possible, puis, pour personnaliser une page de contenu `<head>` élément en écrivant un peu de code dans la page `Page_Load` Gestionnaire d’événements. Nous examinons comment définir par programmation le titre de la page à l’étape 1.

Le balisage illustré à la `<head>` élément ci-dessus inclut également un contrôle ContentPlaceHolder nommé `head`. Ce contrôle ContentPlaceHolder n’est pas nécessaire, comme les pages de contenu peuvent ajouter du contenu personnalisé pour le `<head>` élément par programmation. Il est utile, toutefois, dans les situations où une page de contenu doit ajouter un balisage statique pour le `<head>` élément en tant que le balisage statique peut être ajouté de manière déclarative pour le contrôle de contenu correspondant plutôt que par programmation.

Outre le `<title>` élément et `head` du ContentPlaceHolder, la page maître `<head>` élément doit contenir les `<head>`-niveau balisage qui est commun à toutes les pages. Dans notre site Web, toutes les pages d’utilisent les règles CSS définies dans le `Styles.css` fichier. Par conséquent, nous avons mis à jour le `<head>` élément dans le [ *création d’une disposition de l’échelle du Site avec des Pages maîtres* ](creating-a-site-wide-layout-using-master-pages-vb.md) didacticiel pour inclure un correspondant `<link>` élément. Notre `Site.master` actuel du gabarit `<head>` balisage est indiqué ci-dessous.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Étape 1 : Définir un titre d’une Page de contenu

Titre de la page web est spécifié via le `<title>` élément. Il est important de définir le titre de chaque page avec une valeur appropriée. Lorsque vous visitez une page, son titre s’affiche dans la barre de titre du navigateur. En outre, lors de la création de signets une page, navigateurs utilisent le titre de la page en tant que le nom suggéré pour le signet. En outre, de nombreux moteurs de recherche affichent le titre de la page lors de l’affichage des résultats de la recherche.

> [!NOTE]
> Par défaut, Visual Studio définit le `<title>` élément dans la page maître pour « Page sans titre ». De même, les nouvelles pages ASP.NET ont leur `<title>` trop la valeur « Page sans titre, ». Car il est facile d’oublier de définir le titre de la page à une valeur appropriée, il existe de nombreuses pages sur Internet avec le titre « Page sans titre ». La recherche Google pour les pages web avec ce titre retourne à peu près les 2,460,000 résultats. Même Microsoft est vulnérable à la publication de pages web avec le titre « Page sans titre ». Au moment de la rédaction, une recherche Google signalée 236 ces pages web dans le domaine Microsoft.com.


Une page ASP.NET peut spécifier son titre dans une des manières suivantes :

- En plaçant la valeur directement dans le `<title>` élément
- À l’aide de la `Title` d’attribut dans le `<%@ Page %>` (directive)
- Définition par programmation de la page `Title` propriété à l’aide de code tel que `Page.Title="title"` ou `Page.Header.Title="title"`.

Contenu de pages n’ont pas un `<title>` élément, tel qu’il est défini dans la page maître. Par conséquent, pour définir le titre d’une page de contenu, vous pouvez utiliser la `<%@ Page %>` de directive `Title` attribut ou définir par programme.

### <a name="setting-the-pages-title-declaratively"></a>Définition de façon déclarative titre de la Page

Titre d’une page de contenu permet de façon déclarative par la `Title` attribut de la [ `<%@ Page %>` directive](https://msdn.microsoft.com/library/ydy4x04a.aspx). Cette propriété peut être définie en modifiant directement le `<%@ Page %>` directive ou via la fenêtre Propriétés. Examinons les deux approches.

À partir de la vue de Source, recherchez la `<%@ Page %>` directive, qui est en haut du balisage déclaratif de la page. Le `<%@ Page %>` directive pour `Default.aspx` suit :


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

Le `<%@ Page %>` directive spécifie les attributs spécifiques à la page utilisés par le moteur ASP.NET lors de l’analyse et la compilation de la page. Cela inclut son fichier de page maître, l’emplacement de son fichier de code et son titre, parmi d’autres informations.

Par défaut, lorsque vous créez une nouvelle page de contenu Visual Studio définit le `Title` attribut « Page sans titre ». Modification `Default.aspx`de `Title` d’attribut à partir de la « Page sans titre » à « Didacticiels de Page maître », puis affichez la page via un navigateur. La figure 1 illustre la barre de titre du navigateur, ce qui reflète le nouveau titre de page.


![Barre de titre du navigateur affiche maintenant &quot;didacticiels de Page maître&quot; au lieu de &quot;Page sans titre&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Figure 01**: Barre de titre du navigateur affiche maintenant les « Didacticiels de la Page maître » au lieu de « Page sans titre »


Titre de la page peut également être défini à partir de la fenêtre Propriétés. À partir de la fenêtre Propriétés, sélectionnez le DOCUMENT dans la liste déroulante aux propriétés de charge niveau de la page, ce qui inclut le `Title` propriété. La figure 2 montre la fenêtre Propriétés après `Title` a été défini sur « Didacticiels de Page maître ».


![Vous pouvez configurer le titre de la fenêtre Propriétés, trop](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Figure 02**: Vous pouvez configurer le titre de la fenêtre Propriétés, trop


### <a name="setting-the-pages-title-programmatically"></a>Définition de titre de la Page par programmation

La page maître `<head runat="server">` balisage est traduit en une [ `HtmlHead` classe](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) instance lorsque la page est affichée par le moteur ASP.NET. Le `HtmlHead` classe a un [ `Title` propriété](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) dont la valeur est reflétée dans le rendu `<title>` élément. Cette propriété est accessible à partir de la classe de code-behind d’une page ASP.NET via `Page.Header.Title`; ce même propriété est également accessible `Page.Title`.

Exercez-vous à titre de la page par programme, accédez à la `About.aspx` code-behind de la page de classe et créer un gestionnaire d’événements de la page `Load` événement. Ensuite, définissez le titre de la page « didacticiels de Page maître :: Environ :: *date*», où *date* est la date actuelle. Après avoir ajouté ce code votre `Page_Load` Gestionnaire d’événements doit ressembler à ce qui suit :


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

La figure 3 illustre la barre de titre du navigateur lors de la visite le `About.aspx` page.


![Titre de la Page est définie par programme et inclut la Date actuelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Figure 03**: Titre de la Page est définie par programme et inclut la Date actuelle


## <a name="step-2-automatically-assigning-a-page-title"></a>Étape 2 : Attribuer automatiquement un titre de Page

Comme nous l’avons vu à l’étape 1, le titre d’une page peut être définie de manière déclarative ou par programmation. Si vous oubliez de modifier explicitement le titre pour qu’elle soit plus descriptive, toutefois, votre page aura le titre par défaut, « Page sans titre ». Dans l’idéal, titre de la page est automatiquement défini pour nous dans le cas où nous ne spécifions pas explicitement sa valeur. Par exemple, si lors de l’exécution le titre de la page est « Page sans titre », nous pourrions ont pour titre automatiquement mis à jour pour être le même que le nom de fichier de la page ASP.NET. La bonne nouvelle est qu’avec un peu de travail initial, qu'il est possible d’avoir le titre assigné automatiquement.

Toutes les pages web ASP.NET dérivent la `Page` classe dans l’espace de noms System.Web.UI. Le `Page` classe définit les fonctionnalités minimales nécessaires à une page ASP.NET et expose des propriétés essentielles telles que `IsPostBack`, `IsValid`, `Request`, et `Response`, entre autres. Souvent, chaque page dans une application web nécessite des fonctionnalités ou des fonctionnalités supplémentaires. Une méthode courante de fournir ce consiste à créer une classe de page de base personnalisée. Une classe de page de base personnalisée est une classe que vous créez et qui dérive de la `Page` classe et inclut des fonctionnalités supplémentaires. Une fois que cette classe de base a été créée, vous pouvez avoir à vos pages ASP.NET de dériver à partir de celui-ci (plutôt que la `Page` classe), ce qui offre la fonctionnalité étendue à vos pages ASP.NET.

Dans cette étape, nous créer une page de base qui définit automatiquement le titre de la page au nom de fichier de la page ASP.NET si le titre n'a pas été explicitement défini autrement. Étape 3 examine définissant le titre de la page selon le plan du site.

> [!NOTE]
> Un examen approfondi de la création et l’utilisation des classes de page de base personnalisée est dépasse le cadre de cette série de didacticiels. Pour plus d’informations, consultez [à l’aide d’une classe de Base personnalisée pour les Classes de Code-Behind de vos Pages ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Création de la classe de Page de Base

Notre première tâche consiste à créer une classe de page de base, qui est une classe qui étend la `Page` classe. Commencez par ajouter un `App_Code` dossier à votre projet en cliquant sur le nom du projet dans l’Explorateur de solutions, choisissez Ajouter le dossier ASP.NET, puis en sélectionnant `App_Code`. Ensuite, avec le bouton droit sur le `App_Code` dossier et ajoutez une nouvelle classe nommée `BasePage.vb`. La figure 4 illustre l’Explorateur de solutions après la `App_Code` dossier et `BasePage.vb` classe ont été ajoutés.


![Ajouter un dossier App_Code et une classe nommée BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Figure 04**: Ajouter un `App_Code` dossier et une classe nommée `BasePage`


> [!NOTE]
> Visual Studio prend en charge deux modes de gestion de projet : Projets de Site Web et projets d’Application Web. Le `App_Code` dossier est conçu pour être utilisé avec le modèle de projet de Site Web. Si vous utilisez le modèle de projet d’Application Web, placez le `BasePage.vb` classe dans un dossier nommé autrement que `App_Code`, tel que `Classes`. Pour plus d’informations sur ce sujet, reportez-vous à [migration d’un projet de Site Web à un projet d’Application Web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Étant donné que la page de base personnalisée sert de classe de base pour les classes de code-behind des pages ASP.NET, il doit étendre la `Page` classe.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Chaque fois qu’une page ASP.NET est demandée, elle parcoure une série d’étapes, sanctionné dans la page demandée est rendue en HTML. Nous pouvons exploiter dans une étape en remplaçant le `Page` la classe `OnEvent` (méthode). Pour notre base de page Nous allons définir automatiquement le titre s’il n’a pas été explicitement spécifié par le `LoadComplete` scène (qui, comme vous l’avez peut-être deviné, se produit après le `Load` étape).

Pour ce faire, vous devez remplacer le `OnLoadComplete` (méthode) et entrez le code suivant :


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

Le `OnLoadComplete` méthode commence par déterminer si le `Title` propriété n'a pas encore été définie explicitement. Si le `Title` est propriété `Nothing`, une chaîne vide, ou a la valeur « Page sans titre », elle est affectée au nom de fichier de la page ASP.NET demandée. Le chemin d’accès physique à la page ASP.NET demandée - `C:\MySites\Tutorial03\Login.aspx`, par exemple, est accessible via la `Request.PhysicalPath` propriété. Le `Path.GetFileNameWithoutExtension` méthode est utilisée pour extraire uniquement la partie du nom de fichier, et ce nom de fichier est ensuite assigné à la `Page.Title` propriété.

> [!NOTE]
> Je vous invite à améliorer cette logique pour améliorer le format du titre. Par exemple, si le nom de fichier de la page est `Company-Products.aspx`, le code ci-dessus génère le titre « Produits de la société », mais dans l’idéal, le tiret aurait été remplacé par un espace, comme dans « Produits d’entreprise ». En outre, vous pouvez envisager un espace chaque fois qu’un changement de casse. Autrement dit, envisagez d’ajouter du code qui transforme le nom de fichier `OurBusinessHours.aspx` à un titre de « notre entreprise heures ».


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Avoir les Pages de contenu d’hériter de la classe de Page de Base

Nous devons maintenant mettre à jour les pages ASP.NET dans notre site dériver à partir de la page de base personnalisée (`BasePage`) au lieu du `Page` classe. Pour ce faire, affichez à chaque classe code-behind et modifiez la déclaration de classe à partir de :


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

À :


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Après cela, visitez le site via un navigateur. Si vous visitez une page dont le titre est explicitement défini, tel que `Default.aspx` ou `About.aspx`, le titre spécifié explicitement est utilisé. Si, toutefois, vous visitez une page dont le titre n’a pas été modifié à partir de la valeur par défaut (« Page sans titre »), la classe de page de base définit le titre au nom de fichier de la page.

La figure 5 illustre le `MultipleContentPlaceHolders.aspx` page lorsqu’ils sont affichés via un navigateur. Notez que le titre est précisément de la page Nom du fichier (moins l’extension), « MultipleContentPlaceHolders ».


[![Si un titre n’est pas explicitement spécifié, le nom de fichier de la Page est automatiquement utilisé](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Figure 05**: Si un titre n’est pas explicitement spécifié, le nom de fichier de la Page est automatiquement utilisé ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Étape 3 : Baser le titre de la Page sur le plan du Site

ASP.NET offre une infrastructure de carte de site robuste qui permet aux développeurs de page définir un plan de site hiérarchique dans une ressource externe (par exemple, une table de base de données ou fichier XML), ainsi que les contrôles Web pour afficher des informations sur le plan du site (par exemple, le contrôle SiteMapPath, Menus et contrôles TreeView).

La structure de plan de site est également accessible par programmation à partir de la classe de code-behind d’une page ASP.NET. De cette manière nous pouvons automatiquement définir le titre d’une page au titre de son nœud correspondant dans le plan du site. Nous allons améliorer la `BasePage` classe créée à l’étape 2 afin qu’il offre cette fonctionnalité. Mais tout d’abord, nous devons créer un plan de site pour notre site.

> [!NOTE]
> Ce didacticiel suppose que le lecteur est déjà familiarisé avec l’ASP. Fonctionnalités de mappage de site du NET. Pour plus d’informations sur l’utilisation de la carte de site, consultez ma série d’articles de plusieurs parties, [ASP d’examen. Navigation sur le Site de NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Création du plan du Site

Le système de mappage de site est construit sur le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), qui dissocie le plan du site API à partir de la logique qui sérialise les informations de plan de site entre la mémoire et un magasin persistant. Le .NET Framework est livré avec le [ `XmlSiteMapProvider` classe](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), qui est le fournisseur de plan de site par défaut. Comme son nom l’indique, `XmlSiteMapProvider` utilise un fichier XML comme magasin de mappage de site. Nous allons utiliser ce fournisseur pour la définition de notre plan de site.

Commencez par créer un fichier de mappage de site dans le dossier de racine du site Web nommé `Web.sitemap`. Pour ce faire, avec le bouton droit sur le nom de site Web dans l’Explorateur de solutions, choisissez Ajouter un nouvel élément, sélectionnez le modèle de plan de Site. Assurez-vous que le fichier est nommé `Web.sitemap` et cliquez sur Ajouter.


[![Ajoutez un fichier nommé Web.sitemap au dossier de racine du site Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Figure 06**: Ajouter un fichier nommé `Web.sitemap` au dossier racine du site Web de ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Ajoutez le code XML suivant à la `Web.sitemap` fichier :


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Ce fichier XML définit la structure de plan de site hiérarchique illustrée à la Figure 7.


![Le plan de Site est actuellement composé de trois nœuds](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Figure 07**: Le plan de Site est actuellement composé de trois nœuds


Nous mettrons à jour la structure de plan de site dans les didacticiels futures pendant que nous ajoutons de nouveaux exemples.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>La mise à jour de la Page maître pour inclure les contrôles de Navigation Web

Maintenant que nous disposons d’un plan de site défini, nous allons mettre à jour la page maître pour inclure les contrôles de navigation Web. Plus précisément, nous allons ajouter un contrôle ListView à la colonne de gauche dans la section de leçons qui restitue une liste non triée avec un élément de liste pour chaque nœud défini dans le plan du site.

> [!NOTE]
> Le contrôle ListView est une nouveauté pour ASP.NET version 3.5. Si vous utilisez une version antérieure d’ASP.NET, utilisez plutôt le contrôle du répéteur. Pour plus d’informations sur le contrôle ListView, consultez [ListView à l’aide de ASP.NET 3.5 et les contrôles DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Démarrez en supprimant le balisage existant de la liste non triée de la section de leçons. Ensuite, faites glisser un contrôle ListView à partir de la boîte à outils et déposez-le sous les leçons titre. Le ListView se trouve dans la section données de la boîte à outils, en même temps que les autres contrôles d’affichage : le GridView, DetailsView et FormView. Définir le ListView `ID` propriété `LessonsList`.

À partir de l’Assistant de Configuration de Source de données choisir de lier le ListView à un nouveau contrôle SiteMapDataSource nommé `LessonsDataSource`. Le contrôle SiteMapDataSource retourne la structure hiérarchique du système de site map.


[![Lier un contrôle SiteMapDataSource au contrôle ListView de LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Figure 08**: Lier un contrôle SiteMapDataSource au contrôle ListView LessonsList ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Après avoir créé le contrôle SiteMapDataSource, nous devons définir les modèles de la ListView afin qu’il génère une liste non triée avec un élément de liste pour chaque nœud retourné par le contrôle SiteMapDataSource. Cela peut être accompli à l’aide de la balise de modèle suivante :


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

Le `LayoutTemplate` génère le balisage pour une liste non triée (`<ul>...</ul>`) alors que le `ItemTemplate` restitue chaque élément retourné par SiteMapDataSource comme un élément de liste (`<li>`) qui contient un lien vers la leçon particulier.

Après avoir configuré les modèles de la ListView, visitez le site Web. Comme le montre la Figure 9, la section de leçons contient un seul élément de liste à puces, accueil. Où se trouvent les propos et à l’aide de leçons de contrôles ContentPlaceHolder plusieurs ? SiteMapDataSource est conçue pour retourner un ensemble hiérarchique de données, mais le contrôle ListView peut uniquement afficher un seul niveau de la hiérarchie. Par conséquent, seul le premier niveau de nœuds retourné par SiteMapDataSource s’affiche.


[![La Section leçons contient un seul élément de liste](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Figure 09**: La Section leçons contient un seul élément de liste ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Pour afficher plusieurs niveaux nous pourrions imbriquer plusieurs ListView dans le `ItemTemplate`. Cette technique a été examinée dans le [ *Pages maîtres et Navigation dans les sites* didacticiel](../../data-access/introduction/master-pages-and-site-navigation-vb.md) de mon [fonctionne avec la série de didacticiels de données](../../data-access/index.md). Toutefois, pour cette série de didacticiels notre plan de site contient uniquement un deux niveaux : Accueil (haut niveau) ; et chaque leçon en tant qu’enfant d’accueil. Au lieu de l’élaboration d’un ListView imbriqué, nous pouvons demander à la place à SiteMapDataSource pour ne pas retourner le nœud de démarrage en définissant son [ `ShowStartingNode` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) à `False`. L’effet net est que SiteMapDataSource démarre en renvoyant le deuxième niveau de nœuds de plan de site.

Avec cette modification, le ListView affiche les éléments de liste à puces pour le propos et à l’aide de plusieurs contrôles ContentPlaceHolder leçons, mais omet un élément de liste à puces pour la maison. Pour résoudre ce problème, nous pouvons ajouter explicitement un élément de liste à puces pour la maison dans le `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

En configurant SiteMapDataSource pour omettre le nœud de démarrage et en ajoutant explicitement un élément de liste à puces d’accueil, la section leçons affiche désormais la sortie prévue.


[![La Section leçons contient un élément de liste à puces pour chaque nœud enfant et personnels](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Figure 10**: La Section leçons contient un élément de liste à puces pour chaque nœud enfant et personnels ([cliquez pour afficher l’image en taille réelle](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Définition du titre basé sur le plan du Site

Avec le plan du site en place, nous pouvons mettre à jour notre `BasePage` classe à utiliser le titre spécifié dans le plan du site. Comme nous l’avons fait à l’étape 2, nous voulons uniquement utiliser les titre du nœud de plan de site si le titre de la page n’a pas été défini explicitement par le développeur de pages. Si la page demandée n’a pas défini de manière explicite titre de la page et est introuvable dans le plan du site, puis nous allons revenir à l’aide du nom de fichier de la page demandée (moins l’extension), comme nous l’avons fait à l’étape 2. Figure 11 illustre ce processus de décision.


![En l’Absence d’une explicitement définir titre de la Page, titre du Site carte nœud correspondant est utilisé](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Figure 11**: En l’Absence d’une explicitement définir titre de la Page, titre du Site carte nœud correspondant est utilisé


Mise à jour le `BasePage` la classe `OnLoadComplete` méthode pour inclure le code suivant :


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Comme auparavant, la `OnLoadComplete` méthode commence par déterminer si titre de la page a été défini explicitement. Si `Page.Title` est `Nothing`, une chaîne vide, ou est affectée la valeur « Page sans titre », puis le code affecte automatiquement une valeur à `Page.Title`.

Pour déterminer le titre à utiliser, le code commence par référencer le [ `SiteMap` classe](https://msdn.microsoft.com/library/system.web.sitemap.aspx)de [ `CurrentNode` propriété](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Retourne le [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instance dans le plan de site qui correspond à la page actuellement demandée. En supposant que la page actuellement demandée se trouve dans le plan du site, le `SiteMapNode`de `Title` propriété est affectée à titre de la page. Si la page actuellement demandée n’est pas dans le plan du site, `CurrentNode` retourne `Nothing` et nom de fichier de la page demandée est utilisé comme titre (comme l’a été effectuée à l’étape 2).

La figure 12 illustre le `MultipleContentPlaceHolders.aspx` page lorsqu’ils sont affichés via un navigateur. Étant donné que le titre de cette page n’est pas définie explicitement, titre du son site carte nœud correspondant est utilisé à la place.


![Titre de la MultipleContentPlaceHolders.aspx Page est extraite du plan de Site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Figure 12**: Titre de la MultipleContentPlaceHolders.aspx Page est extraite du plan de Site


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Étape 4 : Ajout d’autres balises spécifiques à la Page à la`<head>`Section

Les étapes 1, 2 et 3 a examiné de personnalisation de la `<title>` élément sur une base de page par page. En plus de `<title>`, le `<head>` est susceptible de contenir `<meta>` éléments et `<link>` éléments. Comme indiqué précédemment dans ce didacticiel, `Site.master`de `<head>` section comprend un `<link>` élément à `Styles.css`. Étant donné que cela `<link>` élément est défini dans la page maître, il est inclus dans le `<head>` section pour toutes les pages de contenu. Mais comment pouvons-nous ajout `<meta>` et `<link>` éléments sur une base de page par page ?

Le moyen le plus simple pour ajouter du contenu propre à la page à la `<head>` section consiste à créer un contrôle ContentPlaceHolder dans la page maître. Nous avons déjà un telle ContentPlaceHolder (nommé `head`). Par conséquent, pour ajouter un objet personnalisé `<head>` balisage, créer un correspondant contrôle dans la page de contenu et de placer le balisage il.

Pour illustrer l’ajout personnalisé `<head>` balisage à une page, nous allons insérer une `<meta>` description, élément pour notre jeu de pages de contenu actuel. Le `<meta>` élément description fournit une brève description sur la page web ; la plupart des moteurs de recherche incorporent cette information dans une certaine forme lors de l’affichage des résultats de la recherche.

Un `<meta>` élément description a la forme suivante :


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Pour ajouter cette balise à une page de contenu, ajoutez le texte ci-dessus pour le contrôle de contenu qui mappe à la page maître `head` ContentPlaceHolder. Par exemple, pour définir un `<meta>` description, élément pour `Default.aspx`, ajoutez le balisage suivant :


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Étant donné que le `head` ContentPlaceHolder n’est pas dans le corps de la page HTML, le balisage ajouté au contrôle de contenu n’est pas affiché dans la vue de conception. Pour voir les `<meta>` description élément visite `Default.aspx` via un navigateur. Une fois que la page a été chargée, afficher la source et notez que le `<head>` section inclut le balisage spécifié dans le contrôle de contenu.

Prenez un moment pour ajouter `<meta>` des éléments de description à `About.aspx`, `MultipleContentPlaceHolders.aspx`, et `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Ajoutant par programme la balise à le`<head>`région

Le `head` ContentPlaceHolder permet d’ajouter de façon déclarative un balisage personnalisé à la page maître `<head>` région. Balisage personnalisée peut également être ajouté par programmation. N’oubliez pas que le `Page` la classe `Header` propriété retourne le `HtmlHead` instance définie dans la page maître (le `<head runat="server">`).

La possibilité d’ajouter du contenu par programmation le `<head>` région est utile lorsque le contenu à ajouter est dynamique. Par exemple il est basé sur l’utilisateur accédant à la page ; peut-être qu’il est extrait à partir d’une base de données. Quelle que soit la raison, vous pouvez ajouter du contenu à la `HtmlHead` en ajoutant des contrôles à ses `Controls` collection comme suit :


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

Le code ci-dessus ajoute le `<meta>` keywords, élément pour le `<head>` région, qui fournit une liste délimitée par des virgules des mots clés qui décrivent la page. Notez que pour ajouter un `<meta>` balise que vous créez un [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) d’instance, définissez son `Name` et `Content` propriétés, puis ajoutez-le à la `Header`de `Controls` collection. De même, pour ajouter par programmation un `<link>` élément, créer un [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) de l’objet, définissez ses propriétés, puis ajoutez-le à la `Header`de `Controls` collection.

> [!NOTE]
> Pour ajouter le balisage arbitraire, créez un [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) de l’instance, définissez son `Text` propriété, puis ajoutez-le à la `Header`de `Controls` collection.


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons un grand nombre de façons d’ajouter `<head>` balisage région sur une base de page par page. Une page maître doit inclure un `HtmlHead` instance (`<head runat="server">`) avec un ContentPlaceHolder. Le `HtmlHead` instance autorise les pages de contenu pour accéder par programmation la `<head>` région et de façon déclarative et par programme, définir la page de titre ; le contrôle ContentPlaceHolder permet de balisage personnalisée à ajouter à la `<head>` section de façon déclarative par le biais d’un contrôle de contenu.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Définition dynamique de titre de la Page dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Examen ASP. Navigation dans les sites de NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Comment utiliser des balises HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Pages maître dans ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [À l’aide d’ASP.NET de 3.5 ListView et contrôles DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [À l’aide d’une classe de Base personnalisée pour les Classes de Code-Behind de vos Pages ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs livres de sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier livre s’intitule [ *Sams Teach vous-même ASP.NET 3.5 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott peut être atteint à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Zack Jones et Suchi Banerjee. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](multiple-contentplaceholders-and-default-content-vb.md)
> [Suivant](urls-in-master-pages-vb.md)
