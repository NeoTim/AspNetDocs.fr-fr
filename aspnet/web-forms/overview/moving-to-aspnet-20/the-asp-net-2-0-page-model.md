---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Le modèle ASP.NET 2.0 Page (fr) Microsoft Docs
author: rick-anderson
description: Dans ASP.NET 1.x, les développeurs avaient le choix entre un modèle de code en ligne et un modèle de code derrière le code. Code-behind pourrait être mis en œuvre en utilisant soit le Src attr ...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 6c2435a06d04209db21fb8e075f68ff0b7a9ef7e
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542857"
---
# <a name="the-aspnet-20-page-model"></a>Le modèle ASP.NET 2.0 Page

par [Microsoft](https://github.com/microsoft)

> Dans ASP.NET 1.x, les développeurs avaient le choix entre un modèle de code en ligne et un modèle de code derrière le code. Le code-out pourrait être mis en œuvre en utilisant @Page l’attribut Src ou l’attribut CodeBehind de la directive. Dans ASP.NET 2.0, les développeurs ont toujours le choix entre le code inline et le code-derrière, mais il ya eu des améliorations significatives au modèle de code-derrière.

Dans ASP.NET 1.x, les développeurs avaient le choix entre un modèle de code en ligne et un modèle de code derrière le code. Le code-out pourrait être mis en œuvre en utilisant @Page l’attribut Src ou l’attribut CodeBehind de la directive. Dans ASP.NET 2.0, les développeurs ont toujours le choix entre le code inline et le code-derrière, mais il ya eu des améliorations significatives au modèle de code-derrière.

## <a name="improvements-in-the-code-behind-model"></a>Améliorations du modèle De code-derrière

Afin de bien comprendre les modifications du modèle de code-derrière dans ASP.NET 2.0, son meilleur pour examiner rapidement le modèle tel qu’il existait dans ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Le modèle Code-Behind en ASP.NET 1.x

Dans ASP.NET 1.x, le modèle de code-derrière se composait d’un fichier ASPX (la forme Web) et d’un fichier de code-derrière contenant du code de programmation. Les deux fichiers ont @Page été connectés à l’aide de la directive dans le fichier ASPX. Chaque contrôle sur la page ASPX avait une déclaration correspondante dans le fichier de code-derrière comme une variable d’instance. Le fichier de code-derrière contenait également du code pour la liaison d’événement et le code généré nécessaire pour le concepteur de Visual Studio. Ce modèle fonctionnait assez bien, mais parce que chaque élément ASP.NET de la page ASPX nécessitait un code correspondant dans le fichier de code, il n’y avait pas de véritable séparation du code et du contenu. Par exemple, si un concepteur a ajouté un nouveau contrôle serveur à un fichier ASPX en dehors de l’IDE Visual Studio, l’application se casserait en raison de l’absence d’une déclaration pour ce contrôle dans le fichier de code.derrière.

## <a name="the-code-behind-model-in-aspnet-20"></a>Le modèle Code-Behind en ASP.NET 2.0

ASP.NET 2.0 améliore considérablement ce modèle. Dans ASP.NET 2.0, le code-derrière est mis en œuvre à l’aide des nouvelles *classes partielles* fournies dans ASP.NET 2.0. La classe de code-derrière dans ASP.NET 2.0 est définie comme une classe partielle signifiant qu’elle ne contient qu’une partie de la définition de classe. La partie restante de la définition de classe est générée dynamiquement par ASP.NET 2.0 en utilisant la page ASPX au moment de l’exécution ou lorsque le site Web est précompilé. Le lien entre le fichier de code et la page ASPX est toujours établi à l’aide de la directive Page. Cependant, au lieu d’un attribut CodeBehind ou Src, ASP.NET 2.0 utilise maintenant l’attribut CodeFile. L’attribut Inherits est également utilisé pour spécifier le nom de classe de la page.

Une directive typique de page pourrait ressembler à ceci :

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Une définition de classe typique dans un fichier ASP.NET 2.0 sous code peut ressembler à ceci :

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> Les langues gérées sont les seules langues gérées qui prennent actuellement en charge les classes partielles. Par conséquent, les développeurs utilisant J ne seront pas en mesure d’utiliser le modèle de code-derrière dans ASP.NET 2.0.

Le nouveau modèle améliore le modèle de code-derrière parce que les développeurs auront maintenant des fichiers de code qui contiennent seulement le code qu’ils ont créé. Il prévoit également une véritable séparation du code et du contenu car il n’y a pas de déclarations variables de cas dans le fichier de code.derrière.

> [!NOTE]
> Étant donné que la classe partielle de la page ASPX est l’endroit où la liaison d’événements a lieu, les développeurs visual Basic peuvent réaliser une légère augmentation des performances en utilisant le mot clé Handles en code-derrière pour lier les événements. Le mot clé N’a pas d’équivalent.

## <a name="new--page-directive-attributes"></a>Nouveaux attributs de directive de page

ASP.NET 2.0 ajoute de nombreux nouveaux attributs à la directive Page. Les attributs suivants sont nouveaux en ASP.NET 2.0.

## <a name="async"></a>Async

L’attribut Async vous permet de configurer la page à exécuter asynchronement. Bien couvrir les pages asynchrone plus tard dans ce module.

## <a name="asynctimeout"></a>AsyncTimeout (en)

Spécifié le délai d’attente pour les pages asynchrones. La valeur par défaut est de 45 secondes.

## <a name="codefile"></a>CodeFile (En)

L’attribut CodeFile remplace l’attribut CodeBehind dans Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass (en)

L’attribut CodeFileBaseClass est utilisé dans les cas où vous souhaitez que plusieurs pages dérivent d’une seule classe de base. En raison de la mise en œuvre de classes partielles en ASP.NET, sans cet attribut, une classe de base qui utilise des champs communs partagés pour référencer les contrôles déclarés dans une page ASPX ne fonctionnerait pas correctement parce que le moteur de compilation ASP.NETs créera automatiquement de nouveaux membres en fonction des contrôles de la page. Par conséquent, si vous voulez une classe de base commune pour deux pages ou plus dans ASP.NET, vous devrez définir spécifier votre classe de base dans l’attribut CodeFileBaseClass, puis tirer chaque classe de pages de cette classe de base. L’attribut CodeFile est également requis lorsque cet attribut est utilisé.

## <a name="compilationmode"></a>CompilationMode

Cet attribut vous permet de définir la propriété CompilationMode de la page ASPX. La propriété CompilationMode est un recensement contenant les valeurs **Toujours,** **Auto**, et **Jamais**. La valeur par défaut est **Toujours**. Le paramètre **Auto** empêchera ASP.NET de compiler dynamiquement la page si possible. L’exclusion des pages de la compilation dynamique augmente les performances. Toutefois, si une page qui est exclue contient ce code qui doit être compilé, une erreur sera lancée lorsque la page est parcourue.

## <a name="enableeventvalidation"></a>ActiveEventValidation

Cet attribut précise si les événements post-retour et rappel sont validés ou non. Lorsque cela est activé, les arguments pour postback ou rappel des événements sont vérifiés pour s’assurer qu’ils proviennent du contrôle du serveur qui les a rendus à l’origine.

## <a name="enabletheming"></a>EnableTheming (en)

Cet attribut précise si ASP.NET thèmes sont utilisés ou non sur une page. La valeur par défaut est **false**. ASP.NET thèmes sont abordés dans [le module 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas (LinePragmas)

Cet attribut précise si des pragmas de ligne doivent être ajoutés pendant la compilation. Les pragmas de ligne sont des options utilisées par les débingiers pour marquer des sections spécifiques de code.

## <a name="maintainscrollpositiononpostback"></a>MaintenirScrollPositionOnPostback

Cet attribut précise si JavaScript est injecté ou non dans la page afin de maintenir la position de défilement entre les postbacks. Cet attribut est **faux** par défaut.

Lorsque cet attribut est **vrai,** ASP.NET &lt;ajoutera un bloc de script&gt; sur postback qui ressemble à ceci:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Notez que le cr pour ce bloc de script est WebResource.axd. Cette ressource n’est pas un chemin physique. Lorsque ce script est demandé, ASP.NET construit dynamiquement le script.

### <a name="masterpagefile"></a>MasterPageFile (en)

Cet attribut spécifie le fichier de la page principale pour la page actuelle. Le chemin peut être relatif ou absolu. Les pages maîtresses sont couvertes dans [le module 4](master-pages.md).

## <a name="stylesheettheme"></a>Feuille de styleTheme

Cet attribut vous permet de remplacer les propriétés d’apparence utilisateur-interface définies par un thème ASP.NET 2.0. Les thèmes sont abordés dans [le module 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Thème

Spécifie le thème de la page. Si une valeur n’est pas spécifiée pour l’attribut StyleSheetTheme, l’attribut Thème remplace tous les styles appliqués aux contrôles sur la page.

## <a name="title"></a>Intitulé

Définit le titre de la page. La valeur spécifiée ici &lt;&gt; apparaîtra dans l’élément titre de la page rendue.

### <a name="viewstateencryptionmode"></a>VoirStateEncryptionMode

Définit la valeur pour l’énumération ViewStateEncryptionMode. Les valeurs disponibles sont **toujours**, **Auto**, et **Jamais**. La valeur par défaut est **Auto**. Lorsque cet attribut est défini à une valeur **d’Auto**, viewstate est crypté est un contrôle lui demande en appelant la méthode **RegisterRequiresViewStateEncryption.**

## <a name="setting-public-property-values-via-the--page-directive"></a>Définir les valeurs foncières par l’intermédiaire de la directive page

Une autre nouvelle capacité de la directive page dans ASP.NET 2.0 est la possibilité de définir la valeur initiale des propriétés publiques d’une classe de base. Supposons, par exemple, que vous avez une propriété publique appelée **SomeText** dans votre classe de base et que vous souhaitez qu’elle soit para paraspéisée à **Bonjour** lorsqu’une page est chargée. Vous pouvez y parvenir en définissant simplement la valeur dans la directive Page comme ainsi:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**L’attribut SomeText** de la directive Page définit la valeur initiale de la propriété SomeText dans la classe de base à *Hello!*. La vidéo ci-dessous est une procédure pas à pas de définir la valeur initiale d’un bien public dans une classe de base en utilisant la directive Page.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Ouvrez la vidéo plein écran](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Nouvelles propriétés publiques de la classe de page

Les propriétés publiques suivantes sont neuves en ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Retourne le chemin application-relatif à la page ou au contrôle. Par exemple, pour une http://app/folder/page.aspxpage située à , la propriété retourne '/dossier/.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Retourne le chemin d’annuaire virtuel relatif à la page ou au contrôle. Par exemple pour une http://app/folder/page.aspxpage située à , la propriété retourne '/dossier/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout (en)

Obtient ou définit le délai d’attente utilisé pour la manipulation de page asynchrone. (Les pages asynchrones seront couvertes plus tard dans ce module.)

## <a name="clientquerystring"></a>ClientQueryString

Une propriété de lecture seulement qui renvoie la partie de chaîne de requête de l’URL demandée. Cette valeur est codée par URL. Vous pouvez utiliser la méthode UrlDecode de la classe HttpServerUtility pour la décoder.

## <a name="clientscript"></a>ClientScript (en)

Cette propriété renvoie un objet ClientScriptManager qui peut être utilisé pour gérer l’émission ASP.NETs de script côté client. (La classe ClientScriptManager est couverte plus tard dans ce module.)

## <a name="enableeventvalidation"></a>ActiveEventValidation

Cette propriété contrôle si oui ou non la validation d’événements est activée pour les événements post-retour et de rappel. Lorsqu’il est activé, les arguments pour publier ou annuler les événements sont vérifiés pour s’assurer qu’ils proviennent du contrôle du serveur qui les a rendus à l’origine.

## <a name="enabletheming"></a>EnableTheming (en)

Cette propriété obtient ou définit un Boolean qui spécifie si oui ou non un ASP.NET thème 2.0 s’applique à la page.

## <a name="form"></a>Formulaire

Cette propriété renvoie le formulaire HTML sur la page ASPX comme objet HtmlForm.

## <a name="header"></a>En-tête

Cette propriété renvoie une référence à un objet HtmlHead qui contient l’en-tête de la page. Vous pouvez utiliser l’objet HtmlHead retourné pour obtenir / définir des feuilles de style, balises Meta, etc.

## <a name="idseparator"></a>IdSeparator (en)

Cette propriété de lecture seulement obtient le caractère qui est employé pour séparer les identifiants de contrôle quand ASP.NET est la construction d’une pièce d’identité unique pour les contrôles sur une page. Elle n'est pas destinée à être utilisée directement à partir du code.

## <a name="isasync"></a>IsAsync

Cette propriété permet des pages asynchrones. Des pages asynchrones sont discutées plus tard dans ce module.

## <a name="iscallback"></a>IsCallback

Cette propriété de lecture seulement retourne **vrai** si la page est le résultat d’un rappel. Les rappels sont discutés plus tard dans ce module.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack (en)

Cette propriété de lecture seulement retourne **vrai** si la page fait partie d’un post-back de page transversale. Les post-arrières de pages transversales sont couverts plus tard dans ce module.

## <a name="items"></a>Éléments

Renvoie une référence à une instance iDictionnaire qui contient tous les objets stockés dans le contexte des pages. Vous pouvez ajouter des éléments à cet objet IDictionary et ils seront à votre disposition tout au long de la durée de vie du contexte.

## <a name="maintainscrollpositiononpostback"></a>MaintenirScrollPositionOnPostBack

Cette propriété contrôle si ASP.NET émet javaScript qui maintient la position de défilement des pages dans le navigateur après un postback se produit. (Les détails de cette propriété ont été discutés plus tôt dans ce module.)

## <a name="master"></a>Master

Cette propriété de lecture seulement renvoie une référence à l’instance MasterPage pour une page à laquelle une page principale a été appliquée.

## <a name="masterpagefile"></a>MasterPageFile (en)

Obtient ou définit le nom de fichier de la page principale pour la page. Cette propriété ne peut être définie que dans la méthode PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength (en)

Cette propriété obtient ou définit la longueur maximale pour les pages état dans les octets. Si la propriété est réglée à un nombre positif, l’état de vue des pages sera divisé en plusieurs champs cachés de sorte qu’il ne dépasse pas le nombre d’octets spécifiés. Si la propriété est un nombre négatif, l’état de vue ne sera pas divisé en morceaux.

## <a name="pageadapter"></a>PageAdapter

Renvoie une référence à l’objet PageAdapter qui modifie la page pour le navigateur demandeur.

## <a name="previouspage"></a>PrécédentPage

Renvoie une référence à la page précédente dans le cas d’un Server.Transfer ou d’un post-page cross..

## <a name="skinid"></a>SkinID SkinID (SkinID)

Spécifie la peau ASP.NET 2.0 à appliquer sur la page.

## <a name="stylesheettheme"></a>Feuille de styleTheme

Cette propriété obtient ou définit la feuille de style qui est appliquée à une page.

## <a name="templatecontrol"></a>Templatecontrol

Renvoie une référence au contrôle contenant de la page.

## <a name="theme"></a>Thème

Obtient ou définit le nom du ASP.NET thème 2.0 appliqué à la page. Cette valeur doit être définie avant la méthode PreInit.

## <a name="title"></a>Intitulé

Cette propriété obtient ou définit le titre de la page telle qu’obtenue à partir de l’en-tête des pages.

## <a name="viewstateencryptionmode"></a>VoirStateEncryptionMode

Obtient ou définit le ViewStateEncryptionMode de la page. Voir une discussion détaillée de cette propriété plus tôt dans ce module.

## <a name="new-protected-properties-of-the-page-class"></a>Nouvelles propriétés protégées de la classe de page

Voici les nouvelles propriétés protégées de la classe Page en ASP.NET 2.0.

## <a name="adapter"></a>Adaptateur

Renvoie une référence au ControlAdapter qui rend la page sur l’appareil qui l’a demandée.

## <a name="asyncmode"></a>AsyncMode

Cette propriété indique si oui ou non la page est traitée asynchrone. Il est destiné à une utilisation par l’heure d’exécution et non pas directement dans le code.

## <a name="clientidseparator"></a>ClientIDSeparator (en)

Cette propriété renvoie le caractère utilisé comme séparateur lors de la création d’ID client unique pour les contrôles. Il est destiné à une utilisation par l’heure d’exécution et non pas directement dans le code.

## <a name="pagestatepersister"></a>PageStatePersister (en)

Cette propriété renvoie l’objet PageStatePersister pour la page. Cette propriété est principalement utilisée par ASP.NET les développeurs de contrôle.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Cette propriété retourne un suffixe unique qui est annexé à la trajectoire de fichier pour la mise en cache des navigateurs. La valeur \_ \_par défaut est ufps et un nombre à 6 chiffres.

## <a name="new-public-methods-for-the-page-class"></a>Nouvelles méthodes publiques pour la classe de page

Les méthodes publiques suivantes sont nouvelles dans la classe Page en ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Cette méthode enregistre les délégués des gestionnaires d’événements pour l’exécution de pages asynchrones. Des pages asynchrones sont discutées plus tard dans ce module.

## <a name="applystylesheetskin"></a>AppliquerStyleSheetSkin

Applique les propriétés dans une feuille de style de pages à la page.

## <a name="executeregisteredasynctasks"></a>ExécuterLes États-Unis

Cette méthode est une tâche asynchrone.

### <a name="getvalidators"></a>GetValidators (en)

Renvoie une collection de validateurs pour le groupe de validation spécifié ou le groupe de validation par défaut si aucun n’est spécifié.

## <a name="registerasynctask"></a>RegisterAsyncTask

Cette méthode enregistre une nouvelle tâche async. Des pages asynchrones sont couvertes plus tard dans ce module.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Cette méthode indique ASP.NET que l’état de contrôle des pages doit être persisté.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Cette méthode indique ASP.NET que les pages viewstate nécessite un cryptage.

## <a name="resolveclienturl"></a>RésoudreClientUrl

Retourne une URL relative qui peut être utilisée pour les demandes de clients pour les images, etc.

## <a name="setfocus"></a>SetFocus

Cette méthode définira la mise au point sur le contrôle spécifié lorsque la page est initialement chargée.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Cette méthode déenregistrera le contrôle qui lui est transmis comme ne nécessitant plus de persistance de l’état de contrôle.

## <a name="changes-to-the-page-lifecycle"></a>Modifications apportées au cycle de vie de la page

Le cycle de vie de la page dans ASP.NET 2.0 n’a pas changé de façon spectaculaire, mais il ya quelques nouvelles méthodes que vous devriez être au courant. Le cycle de vie de la ASP.NET 2,0 pages est décrit ci-dessous.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (Nouveau en ASP.NET 2.0)

L’événement PreInit est la première étape du cycle de vie à laquelle un développeur peut accéder. L’ajout de cet événement permet de modifier de façon programmatique ASP.NET thèmes 2.0, pages maîtresses, propriétés d’accès pour un profil ASP.NET 2.0, etc. Si vous êtes dans un état post-arrière, il est important de se rendre compte que Viewstate n’a pas encore été appliqué aux contrôles à ce stade du cycle de vie. Par conséquent, si un développeur change une propriété d’un contrôle à ce stade, il sera probablement écrasé plus tard dans le cycle de vie des pages.

## <a name="init"></a>Init

L’événement Init n’a pas changé de ASP.NET 1.x. C’est là que vous souhaitez lire ou initialiser les propriétés des contrôles sur votre page. A ce stade, des pages maîtresses, des thèmes, etc. sont déjà appliqués à la page.

## <a name="initcomplete-new-in-20"></a>InitComplete (Nouveau en 2.0)

L’événement InitComplete est convoqué à la fin de l’étape de l’initialisation des pages. À ce stade du cycle de vie, vous pouvez accéder aux contrôles sur la page, mais leur état n’a pas encore été peuplé.

## <a name="preload-new-in-20"></a>PreLoad (Nouveau en 2.0)

Cet événement est appelé après que toutes les données\_post-arrière ont été appliquées et juste avant La charge de page.

## <a name="load"></a>Load

L’événement Load n’a pas changé de ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (Nouveau en 2.0)

L’événement LoadComplete est le dernier événement de l’étape de chargement des pages. À ce stade, toutes les données post-arrière et viewstate ont été appliquées à la page.

## <a name="prerender"></a>Prerender

Si vous souhaitez que l’état de vue soit correctement entretenu pour les contrôles qui sont ajoutés à la page dynamiquement, l’événement PreRender est la dernière occasion de les ajouter.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (Nouveau en 2.0)

Au stade PreRenderComplete, tous les contrôles ont été ajoutés à la page et la page est prête à être rendue. L’événement PreRenderComplete est le dernier événement soulevé avant que les pages viewstate soit sauvée.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (Nouveau en 2.0)

L’événement SaveStateComplete est appelé immédiatement après que tous les points de vue de la page et l’état de contrôle ont été enregistrés. C’est le dernier événement avant que la page soit effectivement rendue au navigateur.

## <a name="render"></a>Rendu

La méthode Render n’a pas changé depuis ASP.NET 1.x. C’est là que le HtmlTextWriter est parasécé et la page est rendue au navigateur.

## <a name="cross-page-postback-in-aspnet-20"></a>Cross-Page Postback en ASP.NET 2.0

Dans ASP.NET 1.x, les post-arrières ont été tenus de poster sur la même page. Les post-arrières de la page transversale n’étaient pas autorisés. ASP.NET 2.0 ajoute la possibilité de poster vers une page différente via l’interface IButtonControl. Tout contrôle qui implémente la nouvelle interface IButtonControl (Button, LinkButton et ImageButton en plus des contrôles personnalisés tiers) peut profiter de cette nouvelle fonctionnalité via l’utilisation de l’attribut PostBackUrl. Le code suivant affiche un contrôle bouton qui poste à une deuxième page.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Lorsque la page est affichée en arrière, la Page qui initie le postback est accessible via la propriété PreviousPage sur la deuxième page. Cette fonctionnalité est implémentée\_via la nouvelle fonction WebForm DoPostBackWithOptions côté client qui ASP.NET 2.0 rend à la page lorsqu’un contrôle renvoie à une page différente. Cette fonction JavaScript est fournie par le nouveau gestionnaire WebResource.axd qui émet du script au client.

La vidéo ci-dessous est une procédure pas à pas d’un post-arrière de la page transversale.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Ouvrez la vidéo plein écran](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Plus de détails sur cross-Page Postbacks

### <a name="viewstate"></a>Viewstate

Vous vous êtes peut-être déjà posé des questions sur ce qui arrive à l’État de vue dès la première page dans un scénario de post-retour de page croisée. Après tout, tout contrôle qui ne met pas en œuvre IPostBackDataHandler persistera son état via viewstate, afin d’avoir accès aux propriétés de ce contrôle sur la deuxième page d’un post-arrière de page croisée, vous devez avoir accès à l’état de vue pour la page. ASP.NET 2.0 s’occupe de ce scénario à l’aide \_ \_d’un nouveau champ caché dans la deuxième page appelée PREVIOUSPAGE. Le \_ \_champ de formulaire PREVIOUSPAGE contient l’état d’accès pour la première page afin que vous puissiez avoir accès aux propriétés de tous les contrôles dans la deuxième page.

### <a name="circumventing-findcontrol"></a>Contourner FindControl

Dans la vidéo pas à pas d’un post-arrière de page croisée, j’ai utilisé la méthode FindControl pour obtenir une référence au contrôle TextBox sur la première page. Cette méthode fonctionne bien à cette fin, mais FindControl est cher et il nécessite la rédaction de code supplémentaire. Heureusement, ASP.NET 2.0 offre une alternative à FindControl à cette fin qui fonctionnera dans de nombreux scénarios. La directive PreviousPageType vous permet d’avoir une référence fortement typée à la page précédente en utilisant soit l’attribut TypeName ou l’attribut VirtualPath. L’attribut TypeName vous permet de spécifier le type de la page précédente tandis que l’attribut VirtualPath vous permet de vous référer à la page précédente à l’aide d’un chemin virtuel. Une fois que vous avez défini la directive PreviousPageType, vous devez ensuite exposer les contrôles, etc. auxquels vous souhaitez autoriser l’accès à l’aide de propriétés publiques.

## <a name="lab-1-cross-page-postback"></a>Lab 1 Cross-Page Postback

Dans ce laboratoire, vous créerez une application qui utilise la nouvelle fonctionnalité de postback cross-page de ASP.NET 2.0.

1. Ouvrez Visual Studio 2005 et créez un nouveau site Web ASP.NET.
2. Ajoutez une nouvelle forme Web appelée page2.aspx.
3. Ouvrez le Default.aspx dans la vue design et ajoutez un contrôle bouton et un contrôle TextBox. 

    1. Donnez au contrôle du bouton un ID de **SubmitButton** et le contrôle TextBox d’un ID de **UserName**.
    2. Définissez la propriété PostBackUrl du bouton à page2.aspx.
4. Ouvrez page2.aspx dans source vue.
5. Ajouter une directive ' PreviousPageType comme indiqué ci-dessous:
6. Ajoutez le code suivant\_à la page Charge de page2.aspx’s code-behind: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Construisez le projet en cliquant sur Construire sur le menu Build.
8. Ajoutez le code suivant au code-derrière pour Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Modifier la\_charge de page dans page2.aspx pour les suivants : 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Créez le projet.
11. Exécutez le projet.
12. Entrez votre nom dans la TextBox et cliquez sur le bouton.
13. Quel est le résultat?

## <a name="asynchronous-pages-in-aspnet-20"></a>Pages asynchrones en ASP.NET 2.0

De nombreux problèmes de contention dans ASP.NET sont causés par la latence des appels externes (tels que les appels de service Web ou de base de données), la latence du fichier IO, etc. Lorsqu’une demande est faite contre une application ASP.NET, ASP.NET utilise l’un de ses fils de travailleur pour le service de cette demande. Cette demande possède ce thread jusqu’à ce que la demande soit terminée et que la réponse ait été envoyée. ASP.NET 2.0 cherche à résoudre les problèmes de latence avec ce genre de problèmes en ajoutant la capacité d’exécuter des pages asynchrone. Cela signifie qu’un thread de travailleur peut démarrer la demande, puis remettre l’exécution supplémentaire à un autre thread, revenant ainsi à la piscine de thread disponible rapidement. Lorsque le fichier IO, appel de base de données, etc. est terminé, un nouveau thread est obtenu à partir du pool de thread pour terminer la demande.

La première étape pour faire exécuter une page asynchronement est de définir **l’attribut Async** de la directive de page comme il le souhaitez:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Cet attribut indique ASP.NET de mettre en œuvre l’IHttpAsyncHandler pour la page.

L’étape suivante consiste à appeler la méthode AddOnPreRenderCompleteAsync à un point dans le cycle de vie de la page avant PreRender. (Cette méthode est généralement\_appelée dans Page Load.) La méthode AddOnPreRenderCompleteAsync prend deux paramètres; un BeginEventHandler et un EndEventHandler. Le BeginEventHandler renvoie un IAsyncResult qui est ensuite passé comme un paramètre à l’EndEventHandler.

La vidéo ci-dessous est une procédure pas à pas d’une demande de page asynchrone.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Ouvrez la vidéo plein écran](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Une page async ne rend pas au navigateur jusqu’à ce que l’EndEventHandler soit terminée. Sans doute, mais que certains développeurs penseront que les demandes async sont similaires aux rappels async. Il est important de se rendre compte qu’ils ne le sont pas. L’avantage aux demandes asynchrones est que le premier thread de travailleur peut être retourné au pool de thread pour servir de nouvelles demandes, réduisant ainsi la contention due à être lié IO, etc.

## <a name="script-callbacks-in-aspnet-20"></a>Script Callbacks in ASP.NET 2.0

Les développeurs Web ont toujours cherché des moyens d’empêcher le scintillement associé à un rappel. Dans ASP.NET 1.x, SmartNavigation était la méthode la plus courante pour éviter les vacillements, mais SmartNavigation a causé des problèmes pour certains développeurs en raison de la complexité de sa mise en œuvre sur le client. ASP.NET 2.0 aborde ce problème avec des rappels de script. Les rappels de script utilisent XMLHttp pour faire des demandes contre le serveur Web via JavaScript. La demande XMLHttp renvoie les données XML qui peuvent ensuite être manipulées via le DOM du navigateur. Le code XMLHttp est caché à l’utilisateur par le nouveau gestionnaire WebResource.axd.

Il ya plusieurs étapes qui sont nécessaires afin de configurer un rappel de script dans ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Étape 1 : Implémentez l’interface ICallbackEventHandler

Afin que ASP.NET reconnaisse votre page comme participant à un rappel de script, vous devez implémenter l’interface ICallbackEventHandler. Vous pouvez le faire dans votre fichier de code-derrière comme si:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Vous pouvez également le faire en utilisant la directive Sur les instruments comme ainsi:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Vous utilisez généralement la directive sur les impléments de l’utilisation du code ASP.NET en ligne.

## <a name="step-2--call-getcallbackeventreference"></a>Étape 2 : Call GetCallbackEventReference

Comme mentionné précédemment, l’appel XMLHttp est encapsulé dans le gestionnaire WebResource.axd. Lorsque votre page est rendue, ASP.NET ajoutera un\_appel à WebForm DoCallback, un script client qui est fourni par WebResource.axd. La fonction\_WebForm DoCallback \_ \_remplace la fonction doPostBack pour un rappel. N’oubliez pas que \_ \_doPostBack soumet le formulaire sur la page. Dans un scénario de rappel, vous voulez \_ \_empêcher un postback, donc doPostBack ne suffira pas.

> [!NOTE]
> \_\_doPostBack est toujours rendu à la page dans un scénario de rappel de script client. Cependant, il n’est pas utilisé pour le rappel.

Les arguments pour\_la fonction WebForm DoCallback côté client sont fournis via la fonction côté serveur GetCallbackEventReference qui serait normalement appelé dans Page\_Load. Un appel typique à GetCallbackEventReference pourrait ressembler à ceci:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> Dans ce cas, cm est un cas de ClientScriptManager. La classe ClientScriptManager sera couverte plus tard dans ce module.

Il existe plusieurs versions surchargées de GetCallbackEventReference. En l’espèce, les arguments sont les suivants :

`this`

Une référence au contrôle où GetCallbackEventReference est appelé. Dans ce cas, c’est la page elle-même.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Un argument de chaîne qui sera transmis du code côté client à l’événement côté serveur. Dans ce cas, Im passant la valeur d’un dropdown appelé ddlCompany.

`ShowCompanyName`

Le nom de la fonction client-côté qui acceptera la valeur de retour (comme chaîne) de l’événement de rappel côté serveur. Cette fonction ne sera appelée que lorsque le rappel côté serveur est réussi. Par conséquent, pour des raisons de robustesse, il est généralement recommandé d’utiliser la version surchargée de GetCallbackEventReference qui prend un argument de chaîne supplémentaire spécifiant le nom d’une fonction côté client à exécuter en cas d’erreur.

`null`

Une chaîne représentant une fonction client-côté qu’il a initiée avant le rappel au serveur. Dans ce cas, il n’y a pas un tel script, de sorte que l’argument est nul.

`true`

Un Boolean spécifiant s’il y a ou non pour effectuer le rappel asynchrone.

L’appel à\_WebForm DoCallback sur le client passera ces arguments. Par conséquent, lorsque cette page est rendue sur le client, ce code ressemblera à ceci :

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Notez que la signature de la fonction sur le client est un peu différente. La fonction côté client passe 5 cordes et un Boolean. La chaîne supplémentaire (qui est nulle dans l’exemple ci-dessus) contient la fonction côté client qui traitera toutes les erreurs de la lecture côté serveur.

## <a name="step-3--hook-the-client-side-control-event"></a>Étape 3 : Accrochez-vous à l’événement de contrôle client-côté

Notez que la valeur de retour de GetCallbackEventReference ci-dessus a été attribuée à une variable de chaîne. Cette chaîne est utilisée pour accrocher un événement côté client pour le contrôle qui initie le rappel. Dans cet exemple, le rappel est initié par un dropdown sur la page, donc je veux accrocher l’événement *OnChange.*

Pour brancher l’événement côté client, il suffit d’ajouter un gestionnaire à la marge du côté du client comme suit:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Rappelons que *cbRef* est la valeur de retour de l’appel à GetCallbackEventReference. Il contient l’appel\_à WebForm DoCallback qui a été montré ci-dessus.

## <a name="step-4--register-the-client-side-script"></a>Étape 4 : Enregistrez le script client-côté

Rappelons que l’appel à GetCallbackEventReference précisait qu’un script côté client appelé **ShowCompanyName** serait exécuté lorsque le rappel côté serveur réussirait. Ce script doit être ajouté à la page à l’aide d’une instance ClientScriptManager. (La classe ClientScriptManager sera discutée plus tard dans ce module.) Vous faites cela comme si:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Étape 5 : Appelez les méthodes de l’interface ICallbackEventHandler

L’ICallbackEventHandler contient deux méthodes que vous devez implémenter dans votre code. Ils sont **RaiseCallbackEvent** et **GetCallbackEvent**.

**RaiseCallbackEvent** prend une ficelle comme argument et ne renvoie rien. L’argument de la chaîne est transmis\_de l’appel côté client à WebForm DoCallback. Dans ce cas, cette valeur est l’attribut de *valeur* de la baisse appelée ddlCompany. Votre code côté serveur doit être placé dans la méthode RaiseCallbackEvent. Par exemple, si votre rappel fait un WebRequest contre une ressource externe, ce code doit être placé dans RaiseCallbackEvent.

**GetCallbackEvent** est responsable du traitement du retour du rappel au client. Il ne prend pas d’arguments et renvoie une chaîne. La chaîne qu’il retourne sera passé comme un argument à la fonction client-côté, dans ce cas *ShowCompanyName*.

Une fois que vous avez terminé les étapes ci-dessus, vous êtes prêt à effectuer un rappel de script dans ASP.NET 2.0.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Ouvrez la vidéo plein écran](the-asp-net-2-0-page-model/_static/callback1.wmv)

Les rappels de script dans ASP.NET sont pris en charge dans n’importe quel navigateur qui prend en charge la prise d’appels XMLHttp. Cela inclut tous les navigateurs modernes utilisés aujourd’hui. Internet Explorer utilise l’objet XMLHttp ActiveX tandis que d’autres navigateurs modernes (y compris le prochain IE 7) utilisent un objet XMLHttp intrinsèque. Pour déterminer de manière programmatique si un navigateur prend en charge les rappels, vous pouvez utiliser la propriété **Request.Browser.SupportCallback.** Cette propriété sera **vraie** si le client demandeur prend en charge les rappels de script.

## <a name="working-with-client-script-in-aspnet-20"></a>Travailler avec Client Script en ASP.NET 2.0

Les scripts clients dans ASP.NET 2.0 sont gérés via l’utilisation de la classe ClientScriptManager. La classe ClientScriptManager permet de suivre les scripts des clients à l’aide d’un type et d’un nom. Cela empêche le même script d’être programmatiquement inséré sur une page plus d’une fois.

> [!NOTE]
> Une fois qu’un script a été enregistré avec succès sur une page, toute tentative ultérieure d’enregistrer le même script se traduira simplement par le script n’est pas enregistré une deuxième fois. Aucun script en double n’est ajouté et aucune exception ne se produit. Pour éviter le calcul inutile, il existe des méthodes que vous pouvez utiliser pour déterminer si un script est déjà enregistré afin que vous ne tentez pas de l’enregistrer plus d’une fois.

Les méthodes du ClientScriptManager doivent être familières à tous les développeurs actuels ASP.NET :

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock (en)

Cette méthode ajoute un script en haut de la page rendue. Ceci est utile pour ajouter des fonctions qui seront explicitement appelées sur le client.

Il existe deux versions surchargées de cette méthode. Trois des quatre arguments sont communs parmi eux. Il s'agit de :

`type (string)`

***L’argument type*** identifie un type pour le script. C’est généralement une bonne idée d’utiliser le type de la page (ceci. GetType()) pour le type.

`key (string)`

***L’argument clé*** est une clé définie par l’utilisateur pour le script. Cela devrait être unique pour chaque script. Si vous essayez d’ajouter un script avec la même clé et le même type de script déjà ajouté, il ne sera pas ajouté.

`script (string)`

***L’argument du script*** est une chaîne contenant le script réel à ajouter. Il est recommandé d’utiliser un StringBuilder pour créer le script, puis d’utiliser la méthode ToString() sur le StringBuilder pour attribuer l’argument du ***script.***

Si vous utilisez le RegisterClientScriptBlock surchargé qui ne prend que&lt;trois&gt; &lt;arguments,&gt;vous devez inclure des éléments de script (script et /script) dans votre script.

Vous pouvez choisir d’utiliser la surcharge de RegisterClientScriptBlock qui prend un quatrième argument. Le quatrième argument est un Boolean qui spécifie si oui ou non ASP.NET devrait ajouter des éléments de script pour vous. Si cet argument est **vrai,** votre script ne doit pas inclure explicitement les éléments de script.

Utilisez la méthode IsClientScriptBlockRegistered pour déterminer si un script a déjà été enregistré. Cela vous permet d’éviter une tentative de ré-enregistrer un script qui a déjà été enregistré.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (Nouveau en 2.0)

L’étiquette RegisterClientScriptInclude crée un bloc de script qui relie à un fichier de script externe. Il a deux surcharges. On prend une clé et une URL. Le second ajoute un troisième argument précisant le type.

Par exemple, le code suivant génère un bloc de script qui relie à jsfunctions.js dans la racine du dossier scripts de l’application:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Ce code produit le code suivant dans la page rendue :

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Le bloc de script est rendu au bas de la page.

Utilisez la méthode IsClientScriptIncludeRegistered pour déterminer si un script a déjà été enregistré. Cela vous permet d’éviter une tentative de réinscriter un script.

## <a name="registerstartupscript"></a>Registerstartupscript

La méthode RegisterStartupScript prend les mêmes arguments que la méthode RegisterClientScriptBlock. Un script enregistré auprès de RegisterStartupScript s’exécute après les charges de la page, mais avant l’événement onLoad côté client. En 1.X, les scripts enregistrés auprès de RegisterStartupScript ont été placés juste avant l’étiquette &lt;de&gt; clôture/formulaire &lt;&gt; tandis que les scripts enregistrés avec RegisterClientScriptBlock ont été placés immédiatement après l’étiquette de formulaire d’ouverture. Dans ASP.NET 2.0, les deux sont placés immédiatement avant la fermeture &lt;/ étiquette de formulaire.&gt;

> [!NOTE]
> Si vous enregistrez une fonction avec RegisterStartupScript, cette fonction ne s’exécutera pas tant que vous ne l’appelez pas explicitement dans le code client.

Utilisez la méthode IsStartupScriptRegistered pour déterminer si un script a déjà été enregistré et éviter une tentative de réenregistrer un script.

## <a name="other-clientscriptmanager-methods"></a>Autres méthodes clientScriptManager

Voici quelques-unes des autres méthodes utiles de la classe ClientScriptManager.

|  <strong>GetCallbackEventReference (en)</strong>   |                                                 Voir les rappels de script plus tôt dans ce module.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink (en)</strong>  |                Obtient une référence JavaScript&lt;(javascript: appel&gt;) qui peut être utilisé pour poster de retour d’un événement côté client.                 |
|  <strong>GetPostBackEventReference (en)</strong>   |                                   Obtient une chaîne qui peut être utilisée pour lancer un poste de retour du client.                                    |
|      <strong>GetWebResourceUrl</strong>       | Renvoie une URL à une ressource intégrée dans un assemblage. Doit être utilisé en conjonction avec <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource (en)</strong> |     Enregistre une ressource Web avec la page. Il s’agit de ressources intégrées dans un assemblage et gérées par le nouveau gestionnaire WebResource.axd.      |
|     <strong>RegisterHiddenField (en anglais seulement)</strong>      |                                                 Enregistre un champ de formulaire caché avec la page.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Enregistre le code côté client qui s’exécute lorsque le formulaire HTML est soumis.                                   |
