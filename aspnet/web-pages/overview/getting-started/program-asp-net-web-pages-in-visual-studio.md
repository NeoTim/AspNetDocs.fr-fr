---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmation ASP.NET Web Pages (Razor) à l’aide de Visual Studio | Microsoft Docs
author: Rick-Anderson
description: Cette annexe explique comment vous pouvez utiliser Visual Studio 2010 ou Visual Web Developer 2010 Express programmation ASP.NET Web Pages avec la syntaxe Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5b8df17ec1021d133579e23cb4f5b0d0f67d4c7c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57058316"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programmation de Pages Web ASP.NET (Razor) à l’aide de Visual Studio
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment vous pouvez utiliser Visual Studio ou Visual Web Developer Express pour le programme des sites Web ASP.NET Web Pages (Razor).
>
> Ce que vous allez apprendre
>
> - Vous devez installer (voire rien) pour travailler avec des Pages Web ASP.NET dans votre version de Visual Studio.
> - Comment ajouter la prise en charge pour les Pages Web ASP.NET pour Visual Web Developer 2010 Express.
> - Comment utiliser les fonctionnalités dans Visual Studio pour travailler avec les pages ASP.NET Razor, y compris IntelliSense et le débogueur.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
>
>
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>
>
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 et WebMatrix 2.


Vous pouvez programmer ASP.NET Web pages avec syntaxe Razor à l’aide de WebMatrix ou nombreux autres éditeurs de code. Vous pouvez également utiliser Microsoft Visual Studio est un environnement complet de développement intégré (IDE) qui fournit un ensemble puissant d’outils pour la création de nombreux types d’applications (pas seulement les sites Web). Pour travailler avec les pages ASP.NET Razor, vous pouvez utiliser [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Deux fonctionnalités particulièrement utiles que Visual Studio fournit pour la programmation avec les pages web ASP.NET Razor sont :

- *IntelliSense*. La fonctionnalité IntelliSense intégrée à Visual Studio est plus complet que IntelliSense dans WebMatrix.
- *Débogueur*. Le débogueur vous permet de résoudre les problèmes de votre code en arrêtant un programme pendant qu’il est en cours d’exécution, examen des variables et parcourir le code ligne par ligne.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>À l’aide de Visual Studio avec différentes Versions d’ASP.NET Web Pages

Pour développer les applications web ASP.NET dans Visual Studio 2017, installez le **ASP.NET et développement web** charge de travail.

Visual Studio 2012 et Visual Studio 2013 incluent la prise en charge pour les Pages Web ASP.NET. (Les packages qui sont nécessaires pour prendre en charge des Pages Web ASP.NET sont installés lorsque vous installez Visual Studio.)

Visual Studio 2010 n’inclut pas prise en charge par défaut pour ASP.NET Web Pages. Pour utiliser les Pages Web ASP.NET avec Visual Studio 2010, vous devez installer le package d’ASP.NET MVC. Pour obtenir des Pages Web ASP.NET 2, vous installez ASP.NET MVC 4.

Le tableau suivant récapitule la prise en charge pour les Pages Web ASP.NET dans les différentes versions de Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Installer ASP.NET MVC 4 | (Inclus) | (Inclus) |
| **Pages Web ASP.NET 3** |  | Mise à jour à ASP.NET Web Pages NuGet via 3 | (Inclus) |

Pour travailler avec Visual Studio 2010, consultez [prise en charge pour les Pages Web ASP.NET dans Visual Studio 2010 installation](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Lancement de Visual Studio à partir de WebMatrix

Si vous avez démarré un projet dans WebMatrix et que vous souhaitez basculer vers Visual Studio, WebMatrix fournit un bouton pour ouvrir facilement le projet dans Visual Studio. Vous devez disposer de Visual Studio installée sur votre ordinateur pour que ce bouton soit activé. L’illustration suivante montre le bouton dans WebMatrix.

![Lancez Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Lorsque vous cliquez sur le bouton, le projet est ouvert dans Visual Studio. Vous pouvez basculer dans les deux sens entre WebMatrix et Visual Studio sans problème. Vous êtes averti si tous les fichiers ont été modifiés dans l’autre environnement et doivent être rechargés pour obtenir les dernières modifications.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Création du site Web ASP.NET Razor dans Visual Studio

Pour créer un site Web ASP.NET Razor dans Visual Studio :

1. Ouvrez Visual Studio.
2. Dans le **fichier** menu, cliquez sur **nouveau Site Web**.

    ![créer un nouveau site web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. Dans le **nouveau Site Web** boîte de dialogue, sélectionnez la langue à utiliser (Visual C# ou Visual Basic).
4. Sélectionnez le **Site de Web ASP.NET (Razor)** modèle.

    ![site de Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Cliquez sur **OK**.

Votre nouveau projet existe et est rempli avec certaines des pages web par défaut pour vous aider à commencer.

### <a name="using-intellisense"></a>Using IntelliSense

Maintenant que vous avez créé un site, vous pouvez voir le fonctionnement d’IntelliSense dans Visual Studio.

1. Dans le site Web que vous venez de créer, ouvrir le *Default.cshtml* page.
2. Après le `<h3>` balises dans la page, tapez `@ServerInfo.` (y compris le point). Notez comment IntelliSense affiche les méthodes disponibles pour le `ServerInfo` helper dans une liste déroulante.

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Sélectionnez le `GetHtml` méthode à partir de la liste et appuyez sur ENTRÉE. IntelliSense remplit automatiquement la méthode. (Comme avec n’importe quelle méthode en C#, vous devez ajouter `()` caractères après la méthode.) Le code complet de le `GetHtml` méthode ressemble à l’exemple suivant :

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Appuyez sur Ctrl + F5 pour exécuter la page. Voici à quoi ressemble la page lorsque affiché dans un navigateur :

    ![page par défaut dans le navigateur](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Fermez le navigateur.

### <a name="using-the-debugger"></a>L’utilisation du débogueur

1. En haut de la *Default.cshtml* page, après la ligne qui commence par `Page.Title`, ajoutez la ligne de code suivante :

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Dans la marge grise de l’éditeur vers la gauche du code, cliquez sur en regard de cette nouvelle ligne afin d’ajouter un *point d’arrêt*. Un point d’arrêt est un marqueur qui indique au débogueur pour interrompre l’exécution du programme à ce stade afin que vous puissiez voir ce qui se passe.

    ![point d’arrêt défini](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Supprimez l’appel à la `ServerInfo.GetHtml` (méthode) et ajoutez un appel à la `@myTime` variable à la place. Cet appel affiche la valeur de temps actuelle qui est retournée par la nouvelle ligne de code.
4. Appuyez sur F5 pour exécuter la page dans le débogueur. La page s’arrête sur le point d’arrêt que vous définissez. L’illustration suivante montre ce que la page se présente comme dans l’éditeur avec le point d’arrêt (en jaune).

    ![point d’arrêt de débogage](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Dans la barre d’outils de débogage, cliquez sur le **pas à pas détaillé** bouton (ou appuyez sur F11) pour exécuter la ligne de code suivante. Chaque fois que vous cliquez sur ce bouton, vous passez l’exécution à la ligne suivante de code.

    ![Pas à pas détaillé de bouton](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Examinez la valeur de la `myTime` variable en maintenant votre pointeur de la souris dessus ou en examinant les valeurs affichées dans le **variables locales** et **pile des appels** windows. Visual Studio affiche la valeur de la variable.

    ![valeur d’afficher l’heure](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Lorsque vous avez terminé l’examen de la variable et le parcours de code, appuyez sur F5 pour continuer l’exécution de la page sans avoir à arrêter au niveau de chaque ligne. Lorsque vous avez terminé de parcourir tout le code, le navigateur affiche la page.

Pour en savoir plus sur le débogueur et le débogage de code dans Visual Studio, consultez [procédure pas à pas : Débogage des Pages Web dans Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>À l’aide de Razor dans les projets ASP.NET MVC avec Visual Studio

La syntaxe Razor est également largement utilisée dans les projets ASP.NET MVC. MVC est un moyen puissant, basé sur des modèles pour créer des sites Web dynamiques. Si votre site ASP.NET Web Pages devient difficile à gérer, vous souhaiterez envisager sa conversion en une application ASP.NET MVC. Pour obtenir un exemple de création d’une application MVC, consultez [mise en route avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>L’installation de la prise en charge des Pages Web ASP.NET dans Visual Studio 2010

Cette section montre comment installer Visual Web Developer Express 2010 et les outils de Pages Web ASP.NET pour Visual Studio.

1. Si vous ne disposez pas de Web Platform Installer, téléchargez-le à partir de l’URL suivante :

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Exécutez le programme Web Platform Installer.
3. Cliquez sur le **produits** onglet.

    ![Onglet produits de WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Recherchez **ASP.NET MVC 4** (pour ASP.NET Web Pages 2) puis cliquez sur **ajouter**. Ces produits incluent Visual Studio tools pour créer des sites Web ASP.NET Razor.

    ![Options d’installation WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Cliquez sur **installer** pour terminer l’installation.
