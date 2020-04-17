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
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="66b88-103">Programmation ASP.NET Pages Web (Razor) à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66b88-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>

<span data-ttu-id="66b88-104"> par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="66b88-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="66b88-105">Cet article explique comment vous pouvez utiliser Visual Studio ou Visual Web Developer Express pour programmer ASP.NET sites Web De pages Web (Razor).</span><span class="sxs-lookup"><span data-stu-id="66b88-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="66b88-106">Ce que vous allez apprendre</span><span class="sxs-lookup"><span data-stu-id="66b88-106">What you'll learn</span></span>
>
> - <span data-ttu-id="66b88-107">Ce que vous devez installer (si quelque chose) pour travailler avec ASP.NET pages Web dans votre version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66b88-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="66b88-108">Comment ajouter un support pour ASP.NET pages Web à Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="66b88-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="66b88-109">Comment utiliser les fonctionnalités de Visual Studio pour travailler avec ASP.NET pages Razor, y compris IntelliSense et le débbugger.</span><span class="sxs-lookup"><span data-stu-id="66b88-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="66b88-110">Versions logicielles utilisées dans le tutoriel</span><span class="sxs-lookup"><span data-stu-id="66b88-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="66b88-111">ASP.NET Pages Web (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="66b88-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="66b88-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="66b88-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="66b88-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="66b88-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="66b88-114">Ce tutoriel fonctionne également avec ASP.NET Pages Web 2, Visual Studio 2012, Visual Studio 2010 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="66b88-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>

<span data-ttu-id="66b88-115">Vous pouvez programmer ASP.NET pages Web avec la syntaxe Razor à l’aide de WebMatrix ou de nombreux autres éditeurs de code.</span><span class="sxs-lookup"><span data-stu-id="66b88-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="66b88-116">Vous pouvez également utiliser Microsoft Visual Studio qui est un environnement de développement intégré (IDE) complet qui fournit un ensemble puissant d’outils pour créer de nombreux types d’applications (pas seulement des sites Web).</span><span class="sxs-lookup"><span data-stu-id="66b88-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="66b88-117">Pour travailler avec ASP.NET pages Razor, vous pouvez utiliser [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="66b88-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="66b88-118">Deux fonctionnalités particulièrement utiles que Visual Studio fournit pour la programmation avec ASP.NET pages Web Razor sont:</span><span class="sxs-lookup"><span data-stu-id="66b88-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="66b88-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="66b88-119">*IntelliSense*.</span></span> <span data-ttu-id="66b88-120">La fonctionnalité IntelliSense intégrée à Visual Studio est plus complète qu’IntelliSense dans WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="66b88-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="66b88-121">*Debugger*.</span><span class="sxs-lookup"><span data-stu-id="66b88-121">*Debugger*.</span></span> <span data-ttu-id="66b88-122">Le débbuggeur vous permet de dépanner votre code en arrêtant un programme pendant qu’il est en cours d’exécution, en examinant les variables et en franchissant le code ligne par ligne.</span><span class="sxs-lookup"><span data-stu-id="66b88-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="66b88-123">Utilisation de Visual Studio avec différentes versions de ASP.NET pages Web</span><span class="sxs-lookup"><span data-stu-id="66b88-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="66b88-124">Pour développer ASP.NET applications web dans Visual Studio 2017, installez la charge de travail **ASP.NET et le développement web.**</span><span class="sxs-lookup"><span data-stu-id="66b88-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="66b88-125">Visual Studio 2012 et Visual Studio 2013 incluent le soutien pour ASP.NET pages Web.</span><span class="sxs-lookup"><span data-stu-id="66b88-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="66b88-126">(Les paquets qui sont nécessaires afin de prendre en charge ASP.NET Pages Web sont installés lorsque vous installez Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="66b88-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="66b88-127">Visual Studio 2010 n’inclut pas la prise en charge par défaut pour ASP.NET pages Web.</span><span class="sxs-lookup"><span data-stu-id="66b88-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="66b88-128">Pour utiliser ASP.NET Pages Web avec Visual Studio 2010, vous devez installer le paquet ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="66b88-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="66b88-129">Pour obtenir ASP.NET Pages Web 2, vous installez ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="66b88-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="66b88-130">Le tableau suivant résume le support de ASP.NET pages Web dans différentes versions de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66b88-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="66b88-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="66b88-131">Visual Studio 2010</span></span> | <span data-ttu-id="66b88-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="66b88-132">Visual Studio 2012</span></span> | <span data-ttu-id="66b88-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="66b88-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="66b88-134">**Pages web ASP.NET 2**</span><span class="sxs-lookup"><span data-stu-id="66b88-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="66b88-135">Installer ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="66b88-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="66b88-136">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="66b88-136">(Included)</span></span> | <span data-ttu-id="66b88-137">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="66b88-137">(Included)</span></span> |
| <span data-ttu-id="66b88-138">**Pages web ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="66b88-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="66b88-139">Mise à jour pour ASP.NET Pages Web 3 via NuGet</span><span class="sxs-lookup"><span data-stu-id="66b88-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="66b88-140">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="66b88-140">(Included)</span></span> |

<span data-ttu-id="66b88-141">Pour travailler avec Visual Studio 2010, voir [Installer le support pour ASP.NET pages Web dans Visual Studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="66b88-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="66b88-142">Lancement de Visual Studio à partir de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="66b88-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="66b88-143">Si vous avez commencé un projet dans WebMatrix et que vous souhaitez passer à Visual Studio, WebMatrix fournit un bouton pour ouvrir facilement le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66b88-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="66b88-144">Vous devez avoir Visual Studio installé sur votre ordinateur pour que ce bouton soit activé.</span><span class="sxs-lookup"><span data-stu-id="66b88-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="66b88-145">L’image suivante montre le bouton dans WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="66b88-145">The following image shows the button in WebMatrix.</span></span>

![lancer Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="66b88-147">Lorsque vous cliquez sur le bouton, le projet est ouvert dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66b88-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="66b88-148">Vous pouvez basculer d’avant en arrière entre WebMatrix et Visual Studio sans aucun problème.</span><span class="sxs-lookup"><span data-stu-id="66b88-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="66b88-149">Vous serez informé si des fichiers ont changé dans l’autre environnement et doivent être rechargés pour obtenir les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="66b88-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="66b88-150">Création d’ASP.NET site razor dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66b88-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="66b88-151">Pour créer un site web ASP.NET Razor dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="66b88-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="66b88-152">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66b88-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="66b88-153">Dans le menu **Du fichier,** cliquez sur **le nouveau site Web**.</span><span class="sxs-lookup"><span data-stu-id="66b88-153">In the **File** menu, click **New Web Site**.</span></span>

    ![créer un nouveau site Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="66b88-155">Dans la boîte de dialogue **du nouveau site Web,** sélectionnez la langue à utiliser (Visual CMD ou Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="66b88-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="66b88-156">Sélectionnez le **modèle ASP.NET site Web (Razor).**</span><span class="sxs-lookup"><span data-stu-id="66b88-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![site de rasoir](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="66b88-158">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="66b88-158">Click **OK**.</span></span>

<span data-ttu-id="66b88-159">Votre nouveau projet existe et est peuplé de pages Web par défaut pour vous aider à démarrer.</span><span class="sxs-lookup"><span data-stu-id="66b88-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="66b88-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="66b88-160">Using IntelliSense</span></span>

<span data-ttu-id="66b88-161">Maintenant que vous avez créé un site, vous pouvez voir comment IntelliSense fonctionne dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66b88-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="66b88-162">Dans le site que vous venez de créer, ouvrez la page *Default.cshtml.*</span><span class="sxs-lookup"><span data-stu-id="66b88-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="66b88-163">Après `<h3>` les balises `@ServerInfo.` dans la page, tapez (y compris le point).</span><span class="sxs-lookup"><span data-stu-id="66b88-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="66b88-164">Remarquez comment IntelliSense affiche `ServerInfo` les méthodes disponibles pour l’aide dans une liste d’abandon.</span><span class="sxs-lookup"><span data-stu-id="66b88-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![Intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="66b88-166">Sélectionnez `GetHtml` la méthode de la liste, puis appuyez sur Enter.</span><span class="sxs-lookup"><span data-stu-id="66b88-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="66b88-167">IntelliSense remplit automatiquement la méthode.</span><span class="sxs-lookup"><span data-stu-id="66b88-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="66b88-168">(Comme avec n’importe quelle méthode `()` en C, vous devez ajouter des caractères après la méthode.) Le code rempli `GetHtml` de la méthode ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="66b88-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="66b88-169">Appuyez sur Ctrl-F5 pour exécuter la page.</span><span class="sxs-lookup"><span data-stu-id="66b88-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="66b88-170">Voici à quoi ressemble la page lorsqu’elle est affichée dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="66b88-170">This is what the page looks like when displayed in a browser:</span></span>

    ![page par défaut dans le navigateur](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="66b88-172">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="66b88-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="66b88-173">Utilisation du Debugger</span><span class="sxs-lookup"><span data-stu-id="66b88-173">Using the Debugger</span></span>

1. <span data-ttu-id="66b88-174">En haut de la page *Default.cshtml,* après `Page.Title`la ligne qui commence par , ajouter la ligne suivante de code:</span><span class="sxs-lookup"><span data-stu-id="66b88-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="66b88-175">Dans la marge grise de l’éditeur à gauche du code, cliquez à côté de cette nouvelle ligne afin d’ajouter un *point d’arrêt*.</span><span class="sxs-lookup"><span data-stu-id="66b88-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="66b88-176">Un point d’arrêt est un marqueur qui indique au débbuggeur d’arrêter d’exécuter le programme à ce moment-là afin que vous puissiez voir ce qui se passe.</span><span class="sxs-lookup"><span data-stu-id="66b88-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![point d’arrêt défini](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="66b88-178">Retirez l’appel à la `ServerInfo.GetHtml` méthode `@myTime` et ajoutez un appel à la variable à sa place.</span><span class="sxs-lookup"><span data-stu-id="66b88-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="66b88-179">Cet appel affiche la valeur de temps actuelle qui est retournée par la nouvelle ligne de code.</span><span class="sxs-lookup"><span data-stu-id="66b88-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="66b88-180">Appuyez sur F5 pour exécuter la page dans le débbugger.</span><span class="sxs-lookup"><span data-stu-id="66b88-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="66b88-181">La page s’arrête sur le point d’arrêt que vous définissez.</span><span class="sxs-lookup"><span data-stu-id="66b88-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="66b88-182">L’image suivante montre à quoi ressemble la page dans l’éditeur avec le point d’arrêt (en jaune).</span><span class="sxs-lookup"><span data-stu-id="66b88-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![point d’arrêt de débogé](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="66b88-184">Dans la barre d’outils Debug, cliquez sur le bouton **Step Into** (ou appuyez sur F11) pour exécuter la ligne suivante de code.</span><span class="sxs-lookup"><span data-stu-id="66b88-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="66b88-185">Chaque fois que vous cliquez sur ce bouton, vous avancez l’exécution vers la ligne de code suivante.</span><span class="sxs-lookup"><span data-stu-id="66b88-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Entrez dans le bouton](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="66b88-187">Examinez la `myTime` valeur de la variable en tenant votre pointeur de souris sur elle ou en inspectant les valeurs affichées dans les **fenêtres locales** et **call Stack.**</span><span class="sxs-lookup"><span data-stu-id="66b88-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="66b88-188">Visual Studio affiche la valeur de la variable.</span><span class="sxs-lookup"><span data-stu-id="66b88-188">Visual Studio display the value of the variable.</span></span>

    ![valeur du temps de spectacle](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="66b88-190">Lorsque vous avez terminé d’examiner la variable et de passer à travers le code, appuyez sur F5 pour continuer à exécuter la page sans s’arrêter à chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="66b88-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="66b88-191">Lorsque vous avez fini de parcourir tout le code, le navigateur affiche la page.</span><span class="sxs-lookup"><span data-stu-id="66b88-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="66b88-192">Pour en savoir plus sur le débbugger et sur la façon de déboguer le code dans Visual Studio, voir [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="66b88-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="66b88-193">Utilisation de Razor dans ASP.NET projets MVC avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66b88-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="66b88-194">La syntaxe Razor est également largement utilisée dans ASP.NET projets MVC.</span><span class="sxs-lookup"><span data-stu-id="66b88-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="66b88-195">MVC est un moyen puissant et basé sur les modèles de créer des sites Web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="66b88-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="66b88-196">Si votre site ASP.NET Pages Web devient difficile à entretenir, vous pouvez envisager de le convertir en une application MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="66b88-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="66b88-197">Pour un exemple de création d’une application MVC, voir [Getting Started avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="66b88-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="66b88-198">Installation d’un support pour ASP.NET pages Web dans Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="66b88-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="66b88-199">Cette section montre comment installer Visual Web Developer Express 2010 et le ASP.NET Web Pages Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66b88-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="66b88-200">Si vous n’avez pas déjà l’installateur de plate-forme Web, téléchargez-le à partir de l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="66b88-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="66b88-201">Exécutez l’installateur de plate-forme Web.</span><span class="sxs-lookup"><span data-stu-id="66b88-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="66b88-202">Cliquez sur l’onglet **Produits.**</span><span class="sxs-lookup"><span data-stu-id="66b88-202">Click the **Products** tab.</span></span>

    ![Onglet Produits WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="66b88-204">Recherchez **ASP.NET MVC 4** (pour ASP.NET Pages Web 2) puis cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="66b88-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="66b88-205">Ces produits comprennent des outils Visual Studio pour la construction de ASP.NET sites Web Razor.</span><span class="sxs-lookup"><span data-stu-id="66b88-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Options d’installation WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="66b88-207">Cliquez sur **Installer** pour terminer l’installation.</span><span class="sxs-lookup"><span data-stu-id="66b88-207">Click **Install** to complete the installation.</span></span>
