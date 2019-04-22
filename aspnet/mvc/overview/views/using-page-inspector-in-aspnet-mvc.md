---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: À l’aide de l’inspecteur de Page dans ASP.NET MVC | Microsoft Docs
author: rick-anderson
description: L’inspecteur de page dans Visual Studio 2012 est un outil de développement web avec un navigateur intégré. Sélectionnez n’importe quel élément dans le navigateur intégré et l’inspecteur de Page i...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: ef0ae42e1c6114849a311164eac242db6dab2b1d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59385795"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a>Utilisation de l'Inspecteur de page dans ASP.NET MVC

par Tim Ammann

> L’inspecteur de page dans Visual Studio 2012 est un outil de développement web avec un navigateur intégré. Sélectionnez n’importe quel élément dans le navigateur intégré et l’inspecteur de Page instantanément met en surbrillance l’élément source et CSS. Vous pouvez parcourir n’importe quelle vue MVC, rapidement rechercher les sources de balisage rendu et utiliser les outils de navigateur directement dans l’environnement Visual Studio.
> 
> [Regardez la vidéo](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Ce didacticiel montre comment activer le Mode d’Inspection et ensuite rapidement localiser et modifier le balisage et CSS dans votre projet web. Ce didacticiel utilise un projet MVC, mais vous pouvez également utiliser l’inspecteur de Page pour [Web Forms](https://go.microsoft.com/?linkid=9802001) et d’autres applications ASP.NET.
> 
> Le didacticiel comporte les sections suivantes :
> 
> - [Composants requis](#_1_prerequisites)
> - [Créer une Application Web](#_2_creating_a)
> - [Utiliser l’inspecteur de Page pour accéder à une vue](#_3_using_page)
> - [Activer le Mode d’Inspection](#_4_inspection_mode)
> - [Utiliser l’inspecteur de Page pour apporter des modifications au balisage](#_5_using_page)
> - [Mode d’inspection et de la fenêtre HTML](#_6_inspection_mode)
> - [Aperçu des modifications dans la fenêtre Styles CSS](#_7_previewing_css)
> - [CSS Auto Sync](#css_auto_sync)
> - [À l’aide du sélecteur de couleurs CSS](#css_color_picker)
> - [Mappage d’éléments de Page dynamique pour JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Prérequis

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Pour obtenir la dernière version de l’inspecteur de Page, utilisez [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) pour installer le Kit de développement logiciel Windows Azure pour .NET 2.0.


Inspecteur de page est fourni avec les outils de développement Web de Microsoft. La dernière version est 1.3. Pour vérifier quelle version vous avez, exécutez Visual Studio, puis sélectionnez **propos de Microsoft Visual Studio** à partir de la **aide** menu.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Créer une Application Web

Tout d’abord, créez une application web que vous utiliserez avec l’inspecteur de Page. Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**. Sur la gauche, développez **Visual C#**, sélectionnez **Web**, puis sélectionnez **ASP.NET MVC 4 Web Application**.

![Nouvelle Application ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Cliquez sur **OK**.

Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **Application Internet**. Laissez **Razor** en tant que le moteur d’affichage par défaut.

![Nouveau projet ASP.NET MVC - Application Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

L’application s’ouvre dans **Source** vue.

![Nouvelle Application ASP.NET MVC dans la vue de Source](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Maintenant que vous avez une application fonctionne avec, vous pouvez utiliser l’inspecteur de Page pour examiner et modifiez-le.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Utiliser l’inspecteur de Page pour accéder à une vue

Dans Visual Studio 2012, vous pouvez cliquer sur n’importe quelle vue dans votre projet, sélectionnez **afficher dans l’inspecteur de Page**, et de trouver l’itinéraire inspecteur de Page et afficher la page.

Dans **l’Explorateur de solutions**, développez le **vues** dossier, puis le **accueil** dossier. Cliquez avec le bouton droit sur le fichier Index.cshtml et choisissez **afficher dans l’inspecteur de Page**.

![Afficher Index.cshtml dans l’inspecteur de Page](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Par défaut, l’inspecteur de Page est ancré en tant que fenêtre sur le côté gauche de l’environnement Visual Studio. Si vous préférez, vous pouvez ancrer ailleurs ou détacher la fenêtre. Voir [Guide pratique pour Organiser et ancrer des fenêtres](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Le volet supérieur de la fenêtre de l’inspecteur de Page affiche la page actuelle dans une fenêtre de navigateur. Le volet inférieur affiche la page dans le balisage HTML, ainsi que certains onglets qui vous permettent d’inspecter les différents aspects de la page. Le volet inférieur est similaire à la [outils de développement F12](https://msdn.microsoft.com/ie/aa740478) dans Internet Explorer.

![Application ASP.NET MVC dans l’inspecteur de Page](using-page-inspector-in-aspnet-mvc/_static/image10.png)

Dans ce didacticiel, vous allez utiliser le **HTML** et **Styles** onglets pour naviguer rapidement et apporter des modifications à l’application.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Mode de EnableInspection

Pour placer l’inspecteur de Page en Mode d’Inspection, cliquez sur le **inspecter** bouton. En Mode d’Inspection, lorsque vous maintenez le pointeur de la souris sur n’importe quelle partie de la page rendue, le code ou le balisage source correspondant est mis en surbrillance.

![Basculer en Mode d’Inspection](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Maintenant, déplacez votre souris sur différentes parties de la page dans l’inspecteur de Page. Comme vous le faites, le pointeur de la souris passe à un signe plus volumineux, et la mise en surbrillance de l’élément en dessous :

![Survole div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Lorsque vous déplacez le pointeur de la souris, Visual Studio met en évidence la syntaxe Razor correspondante dans le fichier source. Si l’élément HTML provient d’un autre fichier source, Visual Studio ouvre automatiquement le fichier.

Dans l’inspecteur de Page, le **HTML** onglet montre le code HTML qui a été généré à partir de la syntaxe Razor. Lorsque vous déplacez le pointeur de la souris, les éléments HTML sont mises en surbrillance. Le **Styles** onglet affiche les règles CSS pour l’élément.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Utiliser l’inspecteur de Page pour apporter des modifications au balisage

Inspecteur de page vous permet de rechercher de balisage dont l’emplacement n’est peut-être pas évident. Ensuite, vous pouvez modifier le balisage et voir les modifications qui en résulte.

Pour voir cela, cliquez sur **inspecter** puis faites défiler vers le bas de la page dans la fenêtre Inspecteur de Page.

Lorsque vous déplacez le pointeur de la souris dans la zone de pied de page, l’inspecteur de Page ouvre le \_Layout.cshtml fichier et met en surbrillance la section de la page de disposition que vous avez sélectionné. Comme vous pouvez le voir, le pied de page sont défini dans le fichier de disposition et pas la vue proprement dite.

![Pied de page](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Maintenant déplacez votre pointeur de la souris sur la ligne avec les informations de copyright <a id="a"> </a>Notez. Dans le \_Layout.cshtml page, la ligne correspondante est mise en surbrillance.

![Ligne de pied de page copyright mis en surbrillance](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Ajouter du texte à la fin de la ligne dans le \_Layout.cshtml fichier.

&lt;p&gt;&amp;copier ; @DateTime.Now.Year -Mon Application ASP.NET MVC est absolument exceptionnel ! &lt;/p&gt;

Maintenant, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour pour afficher les résultats dans la fenêtre de navigateur de l’inspecteur de Page.

![Mon Rocks Application ASP.NET !](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Vous considériez sans doute que le pied de page définies dans Index.cshtml, mais il s’est avéré pour être dans le \_Layout.cshtml et l’inspecteur de Page trouvé pour vous.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Mode d’inspection et de la fenêtre HTML

Ensuite, vous aurez un rapide coup de œil à la fenêtre HTML et comment il mappe les éléments pour vous.

Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.

Cliquez sur la partie supérieure de la page indiquant que « Votre logo ici ». Vous examinez un élément particulier plus en détail, afin de l’affichage dans la fenêtre du navigateur ne change plus lorsque vous déplacez le pointeur de la souris.

Maintenant déplacer le pointeur de la souris vers le **HTML** fenêtre. Lorsque vous déplacez le pointeur de la souris, l’inspecteur de Page décrit l’élément dans le **HTML** fenêtre et met en surbrillance l’élément correspondant dans la fenêtre du navigateur.

![Fenêtre HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Comme auparavant, l’inspecteur de Page ouvre le \_fichier Layout.cshtml pour vous dans un onglet temporaire. Cliquez sur le \_onglet temporaire Layout.cshtml et le balisage correspondant sont mises en surbrillance dans le &lt;en-tête&gt; section pour vous :

![Balisage en surbrillance](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Aperçu des modifications dans la fenêtre Styles CSS

Ensuite, vous allez utiliser l’inspecteur de Page **Styles** fenêtre pour afficher un aperçu des modifications apportées à CSS.

Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.

Dans la fenêtre de navigateur de l’inspecteur de Page, placez le pointeur de la souris sur la section « Page d’accueil » jusqu'à ce que le **div.content-wrapper** étiquette s’affiche.

![Survole div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Cliquez sur une seule fois dans la section div.content-wrapper, et puis déplacez le pointeur de la souris vers le **Styles** fenêtre. Le **Styles** fenêtre affiche toutes les règles CSS pour cet élément. Faites défiler jusqu'à find .featured .content-wrapper du sélecteur de classe. Maintenant désactivez la case à cocher pour la propriété de couleur d’arrière-plan.

![Couleur d’arrière-plan clair](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Notez comment la modification affiche un aperçu d’instantanément dans la fenêtre de navigateur de l’inspecteur de Page.

Sélectionnez la case à cocher à nouveau, puis double-cliquez sur la valeur de propriété et modifiez-le en rouge. La modification s’affiche immédiatement :

![Couleur d’arrière-plan rouge](using-page-inspector-in-aspnet-mvc/_static/image30.png)

Le **Styles** rend fenêtre facilement tester et de prévisualiser les CSS qui change avant de valider les modifications apportées au style de feuille de lui-même.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Synchronisation automatique CSS

> [!NOTE]
> Cette fonctionnalité nécessite la version 1.3 de l’inspecteur de Page.


La fonctionnalité de synchronisation automatique CSS vous permet de modifier directement un fichier CSS et voir les modifications immédiatement dans le navigateur de l’inspecteur de Page.

Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.

Dans le navigateur de l’inspecteur de Page, placez le pointeur de la souris sur la section « Page d’accueil » jusqu'à ce que le **div.content-wrapper** étiquette s’affiche. Cliquez une fois pour sélectionner cet élément.

Le **Styles** fenêtre affiche toutes les règles CSS pour cet élément. Faites défiler jusqu'à find .featured .content-wrapper du sélecteur de classe. Cliquez sur « .featured .content-wrapper ». Inspecteur de page ouvre le fichier CSS qui définit ce style (Site.css) et met en évidence le style CSS correspondant.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Modifiez maintenant la valeur de `background-color` sur « rouge ». La modification s’affiche immédiatement dans le navigateur de l’inspecteur de Page.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>À l’aide du sélecteur de couleurs CSS

L’éditeur CSS dans Visual Studio 2012 a un sélecteur de couleurs qui permet de facilement choisir et insérer des couleurs. Le sélecteur de couleurs inclut une palette de couleurs standard prend en charge les noms de couleurs standard, les codes de hachage, les couleurs RGB, RGBA, HSL et HSLA et gère une liste des couleurs que vous avez utilisé le plus récemment dans le document.

Dans la section précédente, vous avez modifié la valeur de la `background-color` propriété. Pour appeler le sélecteur de couleurs, placez le point d’insertion après le nom de propriété et le type **#** ou **RVB (**.

![La barre de sélecteur de couleurs CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Cliquez sur une couleur pour le sélectionner, ou appuyez sur la touche flèche bas, puis utilisez les touches de direction gauche et droite pour parcourir les couleurs. Lorsque vous visitez une couleur, la valeur hexadécimale correspondante est affiché en aperçu :

![valeur de propriété de couleur d’arrière-plan aperçu](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Si la barre de couleurs n’a pas la couleur exacte voulue, vous pouvez utiliser le sélecteur de couleur pop-bas. Pour l’ouvrir, cliquez sur le double chevron à l’extrémité droite de la barre de couleurs, ou appuyez sur la flèche vers le bas une ou deux fois sur le clavier.

![Sélecteur de couleur CSS Pop-bas](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Cliquez sur une couleur dans la barre verticale sur la droite. Voici un dégradé de cette couleur dans la fenêtre principale. Choisissez une couleur directement à partir de la barre verticale en appuyant sur entrée, ou cliquez sur n’importe quel point dans la fenêtre principale de choisir avec une précision supérieure.

S’il est une couleur à l’écran que vous souhaitez utiliser (il n’a pas à l’intérieur de l’interface utilisateur de Visual Studio), vous pouvez capturer sa valeur à l’aide de l’outil Pipette dans le coin inférieur droit.

Vous pouvez également modifier l’opacité d’une couleur en déplaçant le curseur en bas du sélecteur de couleurs. Cela valeurs RVBA, de couleur de modifications, car le format RVBA peut représenter l’opacité.

Une fois que vous avez choisi une couleur, appuyez sur entrée, puis tapez un point-virgule pour terminer l’entrée de la couleur d’arrière-plan dans le *Site.css* fichier.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>La barre de mise à jour d’inspecteur de Page

Inspecteur de page détecte immédiatement la modification apportée à la *Site.css* de fichiers et affiche une alerte dans une barre de mise à jour.

![Barre de mise à jour](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Pour enregistrer tous vos fichiers et actualisez le navigateur de l’inspecteur de Page, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour. La modification de la couleur de surbrillance apparaît dans le navigateur.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Mappage d’éléments de Page dynamique pour JavaScript

Dans les applications web modernes, éléments de la page sont souvent générées dynamiquement avec JavaScript. Cela signifie que n’est pas statique de balisage (HTML ou Razor) qui correspond à ces éléments de la page.

Avec la version 1.3, l’inspecteur de Page peut mapper maintenant les éléments qui ont été ajoutés dynamiquement à la page sur le code JavaScript correspondant. Pour illustrer cette fonctionnalité, nous allons utiliser le [modèle d’Application à Page unique (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Le modèle SPA nécessite le [ASP.NET et Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) mettre à jour.


Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**. Sur la gauche, développez **Visual C#**, sélectionnez **Web**, puis sélectionnez **ASP.NET MVC 4 Web Application**. Cliquez sur **OK**.

Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **Single Page Application**.

Dans l’Explorateur de solutions, développez le **vues** dossier, puis le **accueil** dossier. Cliquez avec le bouton droit sur le fichier Index.cshtml et choisissez **afficher dans l’inspecteur de Page**.

La première chose qui est affiché dans le navigateur de l’inspecteur de Page est une page de connexion. Cliquez sur « S’abonner » et un nom d’utilisateur et le mot de passe. Une fois que vous vous inscrivez, l’application vous connecte et crée une liste de tâches avec certains exemples d’éléments.

Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection. Dans le navigateur de l’inspecteur de Page, cliquez sur un des éléments d’action. Notez qu’au lieu de la mise en surbrillance en bleu, l’élément est mis en surbrillance en orange, avec « JS » en regard du nom de l’élément. Cela indique que l’élément a été créé dynamiquement via un script.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

En outre, un trait de soulignement orange apparaît sur le **pile des appels** onglet. Cela indique que le **pile des appels** volet a plus d’informations sur l’élément.

Cliquez sur le **pile des appels** onglet. Le **pile des appels** volet affiche la pile des appels pour l’appel de JavaScript qui a créé l’élément. Appels aux bibliothèques externes tels que jQuery sont réduites, afin que vous pouvez facilement voir les appels à votre script de l’application.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Pour afficher la pile complète, y compris les appels à des bibliothèques externes, vous pouvez développer les nœuds nommés « Bibliothèques externes » :

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Si vous cliquez sur un élément dans la pile des appels, Visual Studio ouvre le fichier de code et met en surbrillance le script correspondant.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Voir aussi

[Introduction à ASP.NET MVC 4 avec Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (site Web ASP.net)

[Présentation de l’inspecteur de Page](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vidéo Channel 9)

[Messages d’erreur de l’inspecteur de page](https://go.microsoft.com/?linkid=9813062) (MSDN)
