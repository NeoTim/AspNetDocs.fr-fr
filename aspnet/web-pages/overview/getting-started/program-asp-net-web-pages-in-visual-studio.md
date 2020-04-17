---
uid: web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmation ASP.NET Pages Web (Razor) à l’aide de Visual Studio (fr) Microsoft Docs
author: Rick-Anderson
description: Cette annexe explique comment vous pouvez utiliser Visual Studio 2010 ou Visual Web Developer 2010 Express pour programmer ASP.NET Pages Web avec la syntaxe Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: e586a116fc48a797bdd40befad5851a548282ce6
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542896"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programmation ASP.NET Pages Web (Razor) à l’aide de Visual Studio

 par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment vous pouvez utiliser Visual Studio ou Visual Web Developer Express pour programmer ASP.NET sites Web De pages Web (Razor).
>
> Ce que vous allez apprendre
>
> - Ce que vous devez installer (si quelque chose) pour travailler avec ASP.NET pages Web dans votre version de Visual Studio.
> - Comment ajouter un support pour ASP.NET pages Web à Visual Web Developer 2010 Express.
> - Comment utiliser les fonctionnalités de Visual Studio pour travailler avec ASP.NET pages Razor, y compris IntelliSense et le débbugger.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le tutoriel
>
>
> - ASP.NET Pages Web (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Ce tutoriel fonctionne également avec ASP.NET Pages Web 2, Visual Studio 2012, Visual Studio 2010 et WebMatrix 2.

Vous pouvez programmer ASP.NET pages Web avec la syntaxe Razor à l’aide de WebMatrix ou de nombreux autres éditeurs de code. Vous pouvez également utiliser Microsoft Visual Studio qui est un environnement de développement intégré (IDE) complet qui fournit un ensemble puissant d’outils pour créer de nombreux types d’applications (pas seulement des sites Web). Pour travailler avec ASP.NET pages Razor, vous pouvez utiliser [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Deux fonctionnalités particulièrement utiles que Visual Studio fournit pour la programmation avec ASP.NET pages Web Razor sont:

- *IntelliSense*. La fonctionnalité IntelliSense intégrée à Visual Studio est plus complète qu’IntelliSense dans WebMatrix.
- *Debugger*. Le débbuggeur vous permet de dépanner votre code en arrêtant un programme pendant qu’il est en cours d’exécution, en examinant les variables et en franchissant le code ligne par ligne.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Utilisation de Visual Studio avec différentes versions de ASP.NET pages Web

Pour développer ASP.NET applications web dans Visual Studio 2017, installez la charge de travail **ASP.NET et le développement web.**

Visual Studio 2012 et Visual Studio 2013 incluent le soutien pour ASP.NET pages Web. (Les paquets qui sont nécessaires afin de prendre en charge ASP.NET Pages Web sont installés lorsque vous installez Visual Studio.)

Visual Studio 2010 n’inclut pas la prise en charge par défaut pour ASP.NET pages Web. Pour utiliser ASP.NET Pages Web avec Visual Studio 2010, vous devez installer le paquet ASP.NET MVC. Pour obtenir ASP.NET Pages Web 2, vous installez ASP.NET MVC 4.

Le tableau suivant résume le support de ASP.NET pages Web dans différentes versions de Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **Pages web ASP.NET 2** | Installer ASP.NET MVC 4 | (Inclus) | (Inclus) |
| **Pages web ASP.NET 3** |  | Mise à jour pour ASP.NET Pages Web 3 via NuGet | (Inclus) |

Pour travailler avec Visual Studio 2010, voir [Installer le support pour ASP.NET pages Web dans Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Lancement de Visual Studio à partir de WebMatrix

Si vous avez commencé un projet dans WebMatrix et que vous souhaitez passer à Visual Studio, WebMatrix fournit un bouton pour ouvrir facilement le projet dans Visual Studio. Vous devez avoir Visual Studio installé sur votre ordinateur pour que ce bouton soit activé. L’image suivante montre le bouton dans WebMatrix.

![lancer Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Lorsque vous cliquez sur le bouton, le projet est ouvert dans Visual Studio. Vous pouvez basculer d’avant en arrière entre WebMatrix et Visual Studio sans aucun problème. Vous serez informé si des fichiers ont changé dans l’autre environnement et doivent être rechargés pour obtenir les dernières modifications.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Création d’ASP.NET site razor dans Visual Studio

Pour créer un site web ASP.NET Razor dans Visual Studio :

1. Ouvrez Visual Studio.
2. Dans le menu **Du fichier,** cliquez sur **le nouveau site Web**.

    ![créer un nouveau site Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. Dans la boîte de dialogue **du nouveau site Web,** sélectionnez la langue à utiliser (Visual CMD ou Visual Basic).
4. Sélectionnez le **modèle ASP.NET site Web (Razor).**

    ![site de rasoir](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Cliquez sur **OK**.

Votre nouveau projet existe et est peuplé de pages Web par défaut pour vous aider à démarrer.

### <a name="using-intellisense"></a>Using IntelliSense

Maintenant que vous avez créé un site, vous pouvez voir comment IntelliSense fonctionne dans Visual Studio.

1. Dans le site que vous venez de créer, ouvrez la page *Default.cshtml.*
2. Après `<h3>` les balises `@ServerInfo.` dans la page, tapez (y compris le point). Remarquez comment IntelliSense affiche `ServerInfo` les méthodes disponibles pour l’aide dans une liste d’abandon.

    ![Intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Sélectionnez `GetHtml` la méthode de la liste, puis appuyez sur Enter. IntelliSense remplit automatiquement la méthode. (Comme avec n’importe quelle méthode `()` en C, vous devez ajouter des caractères après la méthode.) Le code rempli `GetHtml` de la méthode ressemble à l’exemple suivant :

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Appuyez sur Ctrl-F5 pour exécuter la page. Voici à quoi ressemble la page lorsqu’elle est affichée dans un navigateur :

    ![page par défaut dans le navigateur](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Fermez le navigateur.

### <a name="using-the-debugger"></a>Utilisation du Debugger

1. En haut de la page *Default.cshtml,* après `Page.Title`la ligne qui commence par , ajouter la ligne suivante de code:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Dans la marge grise de l’éditeur à gauche du code, cliquez à côté de cette nouvelle ligne afin d’ajouter un *point d’arrêt*. Un point d’arrêt est un marqueur qui indique au débbuggeur d’arrêter d’exécuter le programme à ce moment-là afin que vous puissiez voir ce qui se passe.

    ![point d’arrêt défini](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Retirez l’appel à la `ServerInfo.GetHtml` méthode `@myTime` et ajoutez un appel à la variable à sa place. Cet appel affiche la valeur de temps actuelle qui est retournée par la nouvelle ligne de code.
4. Appuyez sur F5 pour exécuter la page dans le débbugger. La page s’arrête sur le point d’arrêt que vous définissez. L’image suivante montre à quoi ressemble la page dans l’éditeur avec le point d’arrêt (en jaune).

    ![point d’arrêt de débogé](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Dans la barre d’outils Debug, cliquez sur le bouton **Step Into** (ou appuyez sur F11) pour exécuter la ligne suivante de code. Chaque fois que vous cliquez sur ce bouton, vous avancez l’exécution vers la ligne de code suivante.

    ![Entrez dans le bouton](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Examinez la `myTime` valeur de la variable en tenant votre pointeur de souris sur elle ou en inspectant les valeurs affichées dans les **fenêtres locales** et **call Stack.** Visual Studio affiche la valeur de la variable.

    ![valeur du temps de spectacle](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Lorsque vous avez terminé d’examiner la variable et de passer à travers le code, appuyez sur F5 pour continuer à exécuter la page sans s’arrêter à chaque ligne. Lorsque vous avez fini de parcourir tout le code, le navigateur affiche la page.

Pour en savoir plus sur le débbugger et sur la façon de déboguer le code dans Visual Studio, voir [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Utilisation de Razor dans ASP.NET projets MVC avec Visual Studio

La syntaxe Razor est également largement utilisée dans ASP.NET projets MVC. MVC est un moyen puissant et basé sur les modèles de créer des sites Web dynamiques. Si votre site ASP.NET Pages Web devient difficile à entretenir, vous pouvez envisager de le convertir en une application MVC ASP.NET. Pour un exemple de création d’une application MVC, voir [Getting Started avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Installation d’un support pour ASP.NET pages Web dans Visual Studio 2010

Cette section montre comment installer Visual Web Developer Express 2010 et le ASP.NET Web Pages Tools for Visual Studio.

1. Si vous n’avez pas déjà l’installateur de plate-forme Web, téléchargez-le à partir de l’URL suivante :

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Exécutez l’installateur de plate-forme Web.
3. Cliquez sur l’onglet **Produits.**

    ![Onglet Produits WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Recherchez **ASP.NET MVC 4** (pour ASP.NET Pages Web 2) puis cliquez sur **Ajouter**. Ces produits comprennent des outils Visual Studio pour la construction de ASP.NET sites Web Razor.

    ![Options d’installation WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Cliquez sur **Installer** pour terminer l’installation.
