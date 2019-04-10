---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Cette annexe vous donne une vue d’ensemble de la programmation avec les pages Web ASP.NET dans Visual Basic, à l’aide de la syntaxe Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: e6b63afb9492e810e19999c7c7ffe074ad510bda
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59406767"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="62d32-103">Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="62d32-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>

<span data-ttu-id="62d32-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="62d32-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="62d32-105">Cet article vous donne une vue d’ensemble de la programmation avec les Pages Web ASP.NET à l’aide de la syntaxe Razor et Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="62d32-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="62d32-106">ASP.NET est la technologie de Microsoft pour les pages web dynamiques en cours d’exécution sur les serveurs web.</span><span class="sxs-lookup"><span data-stu-id="62d32-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="62d32-107">**Ce que vous allez apprendre**:</span><span class="sxs-lookup"><span data-stu-id="62d32-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="62d32-108">Le 8 principaux conseils de mise en route avec ASP.NET Web Pages à l’aide de la syntaxe Razor de la programmation de programmation.</span><span class="sxs-lookup"><span data-stu-id="62d32-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="62d32-109">Concepts de programmation de base que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="62d32-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="62d32-110">Le code de serveur ASP.NET et la syntaxe Razor concerne tous.</span><span class="sxs-lookup"><span data-stu-id="62d32-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="62d32-111">Versions des logiciels</span><span class="sxs-lookup"><span data-stu-id="62d32-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="62d32-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="62d32-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="62d32-113">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="62d32-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="62d32-114">Utilisation de la plupart des exemples de l’utilisation d’ASP.NET Web Pages avec syntaxe Razor c#.</span><span class="sxs-lookup"><span data-stu-id="62d32-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="62d32-115">Mais la syntaxe Razor prend également en charge Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="62d32-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="62d32-116">Pour programmer une page de web ASP.NET dans Visual Basic, vous créez une page web avec un *.vbhtml* extension de nom de fichier, puis ajoutez le code Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="62d32-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="62d32-117">Cet article vous donne une vue d’ensemble de l’utilisation avec le langage Visual Basic et de la syntaxe pour créer des pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="62d32-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="62d32-118">Les modèles de site Web par défaut de Microsoft WebMatrix (**boulangerie**, **galerie de photos**, et **Starter Site**, etc.) sont disponibles dans les versions de c# et Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="62d32-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="62d32-119">Vous pouvez installer les modèles Visual Basic par comme packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="62d32-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="62d32-120">Les modèles de site Web sont installés dans le dossier racine de votre site dans un dossier nommé *Templates Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="62d32-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="62d32-121">Les meilleurs conseils de programmation 8</span><span class="sxs-lookup"><span data-stu-id="62d32-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="62d32-122">Cette section répertorie quelques conseils que vous devez absolument connaître lorsque vous commencez à écrire le code de serveur ASP.NET à l’aide de la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="62d32-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="62d32-123">1. Ajouter du code à une page à l’aide du caractère « @ »</span><span class="sxs-lookup"><span data-stu-id="62d32-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="62d32-124">Le `@` caractère démarre les expressions inline, les blocs d’instruction unique et les blocs d’instructions multiples :</span><span class="sxs-lookup"><span data-stu-id="62d32-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="62d32-125">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-125">The result displayed in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **<span data-ttu-id="62d32-127">Codage HTML</span><span class="sxs-lookup"><span data-stu-id="62d32-127">HTML Encoding</span></span>**
> 
> <span data-ttu-id="62d32-128">Lorsque vous affichez le contenu dans une page à l’aide de la `@` de caractères, comme dans les exemples précédents, ASP.NET encode au format HTML la sortie.</span><span class="sxs-lookup"><span data-stu-id="62d32-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="62d32-129">Cela remplace les caractères HTML réservés (tels que `<` et `>` et `&`) avec des codes qui permettent les caractères à afficher en tant que caractères dans une page web au lieu d’être interprétée comme des balises HTML ou des entités.</span><span class="sxs-lookup"><span data-stu-id="62d32-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="62d32-130">Sans codage HTML, la sortie à partir de votre code de serveur peuvent ne pas s’afficher correctement et peut exposer une page à des risques de sécurité.</span><span class="sxs-lookup"><span data-stu-id="62d32-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="62d32-131">Si votre objectif est de sortie de balisage HTML qui rend les balises en tant que balisage (par exemple `<p></p>` pour un paragraphe ou `<em></em>` pour mettre en évidence de texte), consultez la section [combinant le texte, le balisage et Code dans les blocs de Code](#BM_CombiningTextMarkupAndCode) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="62d32-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="62d32-132">Vous trouverez plus d’informations sur le codage HTML dans [utilisation des formulaires HTML dans les Sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="62d32-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="62d32-133">2. Vous placez des blocs de code avec le Code... Code de fin</span><span class="sxs-lookup"><span data-stu-id="62d32-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="62d32-134">Un bloc de code inclut une ou plusieurs instructions de code et est placé avec les mots clés `Code` et `End Code`.</span><span class="sxs-lookup"><span data-stu-id="62d32-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="62d32-135">Placez l’ouverture `Code` mot clé immédiatement après le `@` caractère &#8212; il ne peut pas être d’un espace blanc entre eux.</span><span class="sxs-lookup"><span data-stu-id="62d32-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="62d32-136">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="62d32-138">3. À l’intérieur d’un bloc, vous obtenez chaque instruction du code avec un saut de ligne</span><span class="sxs-lookup"><span data-stu-id="62d32-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="62d32-139">Dans un bloc de code Visual Basic, chaque instruction se termine par un saut de ligne.</span><span class="sxs-lookup"><span data-stu-id="62d32-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="62d32-140">(Plus loin dans cet article vous verrez un moyen d’encapsuler une instruction de code long en plusieurs lignes si nécessaire.)</span><span class="sxs-lookup"><span data-stu-id="62d32-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="62d32-141">4. Vous utilisez des variables pour stocker des valeurs</span><span class="sxs-lookup"><span data-stu-id="62d32-141">4. You use variables to store values</span></span>

<span data-ttu-id="62d32-142">Vous pouvez stocker des valeurs dans un *variable*, y compris les chaînes, des chiffres et des dates, etc. Vous créez une variable en utilisant le `Dim` mot clé.</span><span class="sxs-lookup"><span data-stu-id="62d32-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="62d32-143">Vous pouvez insérer des valeurs des variables directement dans une page à l’aide `@`.</span><span class="sxs-lookup"><span data-stu-id="62d32-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="62d32-144">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="62d32-146">5. Vous placez les valeurs de chaîne littérale entre guillemets doubles</span><span class="sxs-lookup"><span data-stu-id="62d32-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="62d32-147">Un *chaîne* est une séquence de caractères qui sont traitées en tant que texte.</span><span class="sxs-lookup"><span data-stu-id="62d32-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="62d32-148">Pour spécifier une chaîne, mettez-le entre guillemets doubles :</span><span class="sxs-lookup"><span data-stu-id="62d32-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="62d32-149">Pour incorporer des guillemets doubles dans une valeur de chaîne, insérez deux caractères de guillemet double.</span><span class="sxs-lookup"><span data-stu-id="62d32-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="62d32-150">Si vous souhaitez que le caractère guillemet double d’apparaître une fois dans la sortie de page, entrez-le en tant que `""` au sein de la proposition de prix de chaîne et si vous souhaitez qu’il apparaisse à deux reprises, entrez-le en tant que `""""` dans la chaîne entre guillemets.</span><span class="sxs-lookup"><span data-stu-id="62d32-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="62d32-151">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="62d32-153">6. Code Visual Basic n’est pas sensible à la casse</span><span class="sxs-lookup"><span data-stu-id="62d32-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="62d32-154">Le langage Visual Basic n’est pas sensible à la casse.</span><span class="sxs-lookup"><span data-stu-id="62d32-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="62d32-155">Mots clés de programmation (telles que `Dim`, `If`, et `True`) et les noms de variable (comme `myString`, ou `subTotal`) peuvent être écrites dans tous les cas.</span><span class="sxs-lookup"><span data-stu-id="62d32-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="62d32-156">Les lignes de code suivantes assigner une valeur à la variable `lastname` à l’aide d’une minuscule nommez, puis affichez la valeur de la variable à la page à l’aide d’un nom en majuscules.</span><span class="sxs-lookup"><span data-stu-id="62d32-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="62d32-157">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="62d32-159">7. Une grande partie de votre codage implique l’utilisation des objets</span><span class="sxs-lookup"><span data-stu-id="62d32-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="62d32-160">Un objet représente une chose que vous pouvez programmer avec &#8212; une page, une zone de texte, un fichier, une image, une requête web, un message électronique, un enregistrement de client (ligne de base de données), etc. Objets ont des propriétés qui décrivent leurs caractéristiques &#8212; un objet de zone de texte a un `Text` propriété, un objet de requête a un `Url` propriété, un message électronique a un `From` propriété et un objet customer a une `FirstName` propriété.</span><span class="sxs-lookup"><span data-stu-id="62d32-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="62d32-161">Objets possèdent également des méthodes qui sont le &quot;verbes&quot; qu’ils peuvent effectuer.</span><span class="sxs-lookup"><span data-stu-id="62d32-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="62d32-162">Exemples incluent un objet fichier `Save` (méthode), un objet image `Rotate` (méthode) et un objet de messagerie `Send` (méthode).</span><span class="sxs-lookup"><span data-stu-id="62d32-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="62d32-163">Vous utiliserez souvent le `Request` champs d’objet, ce qui vous donne des informations telles que les valeurs du formulaire sur la page (zones de texte, etc.), le type de navigateur fait la demande, l’URL de la page, de l’identité de l’utilisateur, etc. Cet exemple montre comment accéder aux propriétés de la `Request` objet et comment appeler le `MapPath` méthode de la `Request` objet, ce qui vous donne le chemin d’accès absolu de la page sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="62d32-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="62d32-164">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="62d32-166">8. Vous pouvez écrire du code qui prend des décisions</span><span class="sxs-lookup"><span data-stu-id="62d32-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="62d32-167">Une fonctionnalité clé des pages web dynamiques est que vous pouvez déterminer ce qu’il faut faire en fonction des conditions.</span><span class="sxs-lookup"><span data-stu-id="62d32-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="62d32-168">L’approche la plus courante pour ce faire est avec la `If` instruction (et éventuellement `Else` instruction).</span><span class="sxs-lookup"><span data-stu-id="62d32-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="62d32-169">L’instruction `If IsPost` est un moyen rapide de la rédaction de `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="62d32-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="62d32-170">Avec `If` instructions, il existe plusieurs façons pour tester les conditions, la répétition des blocs de code, et ainsi de suite, qui sont décrits plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="62d32-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="62d32-171">Le résultat affiché dans un navigateur (après avoir cliqué sur **envoyer**) :</span><span class="sxs-lookup"><span data-stu-id="62d32-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **<span data-ttu-id="62d32-173">HTTP GET et POST méthodes et la propriété IsPost</span><span class="sxs-lookup"><span data-stu-id="62d32-173">HTTP GET and POST Methods and the IsPost Property</span></span>**
> 
> <span data-ttu-id="62d32-174">Le protocole utilisé pour les pages web (HTTP) prend en charge un nombre très limité de méthodes (&quot;verbes&quot;) qui sont utilisés pour effectuer des demandes au serveur.</span><span class="sxs-lookup"><span data-stu-id="62d32-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="62d32-175">Les deux plus courantes sont GET, qui est utilisé pour lire une page, et POST, ce qui est utilisé pour envoyer une page.</span><span class="sxs-lookup"><span data-stu-id="62d32-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="62d32-176">En règle générale, la première fois qu’un utilisateur demande une page, la page est demandée à l’aide de GET.</span><span class="sxs-lookup"><span data-stu-id="62d32-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="62d32-177">Si l’utilisateur remplit un formulaire, puis sur **envoyer**, le navigateur envoie une demande POST vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="62d32-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="62d32-178">Dans la programmation web, il est souvent utile de savoir si une page est demandée sous la forme d’une opération GET ou un billet afin que vous sachiez comment traiter la page.</span><span class="sxs-lookup"><span data-stu-id="62d32-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="62d32-179">Dans ASP.NET Web Pages, vous pouvez utiliser le `IsPost` propriété pour déterminer si une requête est une opération GET ou POST.</span><span class="sxs-lookup"><span data-stu-id="62d32-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="62d32-180">Si la demande est une publication, le `IsPost` propriété retournera la valeur true, et vous pouvez effectuer les opérations en lecture les valeurs des zones de texte sur un formulaire.</span><span class="sxs-lookup"><span data-stu-id="62d32-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="62d32-181">Vous verrez de nombreux exemples vous montrent comment traiter la page différemment selon la valeur de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="62d32-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="62d32-182">Un exemple de Code Simple</span><span class="sxs-lookup"><span data-stu-id="62d32-182">A Simple Code Example</span></span>

<span data-ttu-id="62d32-183">Cette procédure vous montre comment créer une page qui illustre des techniques de programmation de base.</span><span class="sxs-lookup"><span data-stu-id="62d32-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="62d32-184">Dans l’exemple, vous créez une page qui permet aux utilisateurs d’entrer des deux nombres, puis il les ajoute et affiche le résultat.</span><span class="sxs-lookup"><span data-stu-id="62d32-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="62d32-185">Dans votre éditeur, créez un nouveau fichier et nommez-le *AddNumbers.vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="62d32-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="62d32-186">Copiez le code et le balisage suivant dans la page, en remplaçant tout élément déjà dans la page.</span><span class="sxs-lookup"><span data-stu-id="62d32-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="62d32-187">Voici quelques éléments de noter :</span><span class="sxs-lookup"><span data-stu-id="62d32-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="62d32-188">Le `@` caractère démarre le premier bloc de code dans la page, et il précède le `totalMessage` variable incorporé vers le bas.</span><span class="sxs-lookup"><span data-stu-id="62d32-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="62d32-189">Le bloc en haut de la page est placé dans `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="62d32-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="62d32-190">Les variables `total`, `num1`, `num2`, et `totalMessage` stocker plusieurs nombres et une chaîne.</span><span class="sxs-lookup"><span data-stu-id="62d32-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="62d32-191">La valeur de littéral de chaîne assignée à la `totalMessage` variable est entre guillemets doubles.</span><span class="sxs-lookup"><span data-stu-id="62d32-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="62d32-192">Étant donné que code Visual Basic n’est pas la casse, lorsque le `totalMessage` variable est utilisée au bas de la page, son nom ne doit correspondre à l’orthographe de la déclaration de variable en haut de la page.</span><span class="sxs-lookup"><span data-stu-id="62d32-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="62d32-193">La casse n’a pas d’importance.</span><span class="sxs-lookup"><span data-stu-id="62d32-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="62d32-194">L’expression `num1.AsInt()`  +  `num2.AsInt()` montre comment utiliser des méthodes et des objets.</span><span class="sxs-lookup"><span data-stu-id="62d32-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="62d32-195">Le `AsInt` méthode sur chaque variable convertit la chaîne entrée par un utilisateur à un nombre entier (entier) qui peut être ajouté.</span><span class="sxs-lookup"><span data-stu-id="62d32-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="62d32-196">Le `<form>` balise inclut un `method="post"` attribut.</span><span class="sxs-lookup"><span data-stu-id="62d32-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="62d32-197">Spécifie que lorsque l’utilisateur clique sur **ajouter**, la page sera être envoyée au serveur à l’aide de la méthode HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="62d32-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="62d32-198">Lorsque la page est envoyée, le code `If IsPost` prend la valeur true et l’instruction conditionnelle de code s’exécute, affiche le résultat de l’ajout de nombres.</span><span class="sxs-lookup"><span data-stu-id="62d32-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="62d32-199">Enregistrez la page et l’exécuter dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="62d32-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="62d32-200">(Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.) Entrez deux nombres entiers, puis activez la **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="62d32-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="62d32-202">Syntaxe et le langage Visual Basic</span><span class="sxs-lookup"><span data-stu-id="62d32-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="62d32-203">Vous avez vu précédemment un exemple de base de la création d’une page web ASP.NET et comment vous pouvez ajouter du code de serveur à la balise HTML.</span><span class="sxs-lookup"><span data-stu-id="62d32-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="62d32-204">Ici, vous allez apprendre les principes fondamentaux de l’utilisation de Visual Basic pour écrire du code de serveur ASP.NET à l’aide de la syntaxe Razor &#8212; , autrement dit, les règles langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="62d32-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="62d32-205">Si vous êtes familiarisé avec la programmation (surtout si vous avez utilisé le C, C++, c#, Visual Basic ou JavaScript), une grande partie de ce que vous lire ici sera familier.</span><span class="sxs-lookup"><span data-stu-id="62d32-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="62d32-206">Vous devrez probablement vous familiariser uniquement avec le code de WebMatrix est ajouté à balisage dans *.vbhtml* fichiers.</span><span class="sxs-lookup"><span data-stu-id="62d32-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="62d32-207">Combinaison de texte, balisage et code dans les blocs de code</span><span class="sxs-lookup"><span data-stu-id="62d32-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="62d32-208">Dans les blocs de code serveur, vous souhaiterez souvent de sortie texte et des balises à la page.</span><span class="sxs-lookup"><span data-stu-id="62d32-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="62d32-209">Si un bloc de code de serveur contient du texte non code, mais qui au lieu de cela doit être restitué en l’état, ASP.NET doit être en mesure de distinguer ce texte à partir du code.</span><span class="sxs-lookup"><span data-stu-id="62d32-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="62d32-210">Pour ce faire, plusieurs méthodes sont possibles.</span><span class="sxs-lookup"><span data-stu-id="62d32-210">There are several ways to do this.</span></span>

- <span data-ttu-id="62d32-211">Placez le texte dans un élément de bloc HTML comme `<p></p>` ou `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="62d32-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="62d32-212">L’élément HTML peut inclure de texte, des éléments HTML supplémentaires et des expressions de code serveur.</span><span class="sxs-lookup"><span data-stu-id="62d32-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="62d32-213">Lorsque ASP.NET voit la balise HTML d’ouverture (par exemple, `<p>`), il restitue tout ce que l’élément et son contenu sous la forme est dans le navigateur (et résout les expressions de code de serveur).</span><span class="sxs-lookup"><span data-stu-id="62d32-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="62d32-214">Utilisez le `@:` opérateur ou la `<text>` élément.</span><span class="sxs-lookup"><span data-stu-id="62d32-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="62d32-215">Le `@:` génère une ligne unique de contenu contenant du texte brut ou des balises HTML sans correspondance ; le `<text>` élément englobe plusieurs lignes de sortie.</span><span class="sxs-lookup"><span data-stu-id="62d32-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="62d32-216">Ces options sont utiles lorsque vous ne souhaitez pas restituer un élément HTML dans le cadre de la sortie.</span><span class="sxs-lookup"><span data-stu-id="62d32-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="62d32-217">L’exemple suivant répète l’exemple précédent, mais utilise une seule paire de `<text>` balises pour encadrer le texte à afficher.</span><span class="sxs-lookup"><span data-stu-id="62d32-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="62d32-218">Dans l’exemple suivant, le `<text>` et `</text>` balises placer trois lignes, disposant de certaines relation contenant-contenu de texte et de des balises HTML sans correspondance (`<br />`), ainsi que le code serveur et de mise en correspondance des balises HTML.</span><span class="sxs-lookup"><span data-stu-id="62d32-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="62d32-219">Là encore, peut également faire précéder chaque ligne individuellement avec la `@:` opérateur ; les deux fonctionnent de façon.</span><span class="sxs-lookup"><span data-stu-id="62d32-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="62d32-220">Sortie de texte comme indiqué dans cette section &#8212; à l’aide d’un élément HTML, le `@:` opérateur, ou la `<text>` élément &#8212; ASP.NET n’encoder en HTML la sortie.</span><span class="sxs-lookup"><span data-stu-id="62d32-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="62d32-221">(Comme indiqué précédemment, ASP.NET n’encode pas la sortie des expressions de code serveur et les blocs de code de serveur qui sont précédés `@`, sauf dans les cas spéciales indiquées dans cette section.)</span><span class="sxs-lookup"><span data-stu-id="62d32-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="62d32-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="62d32-222">Whitespace</span></span>

<span data-ttu-id="62d32-223">L’instruction n’affectent pas les espaces supplémentaires dans une instruction (et en dehors d’un littéral de chaîne) :</span><span class="sxs-lookup"><span data-stu-id="62d32-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="62d32-224">Dernières instructions longues sur plusieurs lignes</span><span class="sxs-lookup"><span data-stu-id="62d32-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="62d32-225">Vous pouvez interrompre une instruction de code long en plusieurs lignes en utilisant le caractère de soulignement `_` (qui, dans Visual Basic est appelé le *caractère de continuation*) après chaque ligne de code.</span><span class="sxs-lookup"><span data-stu-id="62d32-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="62d32-226">Pour arrêter une instruction sur la ligne suivante, ajoutez un espace, puis le caractère de continuation à la fin de la ligne.</span><span class="sxs-lookup"><span data-stu-id="62d32-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="62d32-227">L’instruction continue sur la ligne suivante.</span><span class="sxs-lookup"><span data-stu-id="62d32-227">Continue the statement on the next line.</span></span> <span data-ttu-id="62d32-228">Vous pouvez encapsuler des instructions sur autant de lignes que vous avez besoin améliorer la lisibilité.</span><span class="sxs-lookup"><span data-stu-id="62d32-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="62d32-229">Les instructions suivantes sont les mêmes :</span><span class="sxs-lookup"><span data-stu-id="62d32-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="62d32-230">Toutefois, vous ne pouvez pas encapsuler une ligne au milieu d’un littéral de chaîne.</span><span class="sxs-lookup"><span data-stu-id="62d32-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="62d32-231">L’exemple suivant ne fonctionne pas :</span><span class="sxs-lookup"><span data-stu-id="62d32-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="62d32-232">Pour combiner une longue chaîne répartie sur plusieurs lignes, comme le code ci-dessus, vous devez utiliser le *opérateur de concaténation* (`&`), comme vous le verrez plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="62d32-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="62d32-233">Commentaires de code</span><span class="sxs-lookup"><span data-stu-id="62d32-233">Code comments</span></span>

<span data-ttu-id="62d32-234">Commentaires vous permettent de rédiger des notes pour vous-même ou d’autres personnes.</span><span class="sxs-lookup"><span data-stu-id="62d32-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="62d32-235">Les commentaires de syntaxe Razor sont précédés de `@*` et se terminer par `*@`.</span><span class="sxs-lookup"><span data-stu-id="62d32-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="62d32-236">Dans les blocs de code, vous pouvez utiliser les commentaires de la syntaxe Razor, ou vous pouvez utiliser des caractères de commentaire Visual Basic ordinaires, qui est un guillemet (`'`) ajouté devant chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="62d32-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="62d32-237">Variables</span><span class="sxs-lookup"><span data-stu-id="62d32-237">Variables</span></span>

<span data-ttu-id="62d32-238">Une variable est un objet nommé qui vous permet de stocker des données.</span><span class="sxs-lookup"><span data-stu-id="62d32-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="62d32-239">Vous pouvez nommer les variables, mais le nom doit commencer par un caractère alphabétique et il ne peut pas contenir d’espaces blancs ou des caractères réservés.</span><span class="sxs-lookup"><span data-stu-id="62d32-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="62d32-240">En Visual Basic, comme vous l’avez vu précédemment, la casse des lettres dans un nom de variable importe peu.</span><span class="sxs-lookup"><span data-stu-id="62d32-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="62d32-241">Variables et types de données</span><span class="sxs-lookup"><span data-stu-id="62d32-241">Variables and data types</span></span>

<span data-ttu-id="62d32-242">Une variable peut avoir un type de données spécifique, ce qui indique le type de données est stocké dans la variable.</span><span class="sxs-lookup"><span data-stu-id="62d32-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="62d32-243">Vous pouvez avoir des variables de chaîne qui stockent des valeurs de chaîne (comme &quot;Hello world&quot;), les variables de type entier qui stockent des valeurs de nombre entier (par exemple, 3 ou 79) et les variables de date qui stockent des valeurs de date dans une variété de formats (par exemple, 4/12/2012 ou mars 2009 ).</span><span class="sxs-lookup"><span data-stu-id="62d32-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="62d32-244">Et il existe de nombreux autres types de données que vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="62d32-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="62d32-245">Toutefois, vous n’êtes pas obligé de spécifier un type pour une variable.</span><span class="sxs-lookup"><span data-stu-id="62d32-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="62d32-246">Dans la plupart des cas ASP.NET peut déterminer le type en fonction de la façon dont les données dans la variable sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="62d32-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="62d32-247">(Il peut arriver que vous devez spécifier un type ; vous verrez d’exemples où cela est vrai.)</span><span class="sxs-lookup"><span data-stu-id="62d32-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="62d32-248">Pour déclarer une variable sans spécification d’un type, utilisez `Dim` ainsi que le nom de variable (par exemple, `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="62d32-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="62d32-249">Pour déclarer une variable avec un type, utilisez `Dim` ainsi que le nom de variable, suivi par `As` , puis du nom de type (par exemple, `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="62d32-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="62d32-250">L’exemple suivant illustre certaines expressions inline qui utilisent les variables dans une page web.</span><span class="sxs-lookup"><span data-stu-id="62d32-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="62d32-251">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="62d32-253">Conversion et le test des types de données</span><span class="sxs-lookup"><span data-stu-id="62d32-253">Converting and testing data types</span></span>

<span data-ttu-id="62d32-254">Bien qu’ASP.NET peut généralement déterminer automatiquement un type de données, parfois, il ne peut pas.</span><span class="sxs-lookup"><span data-stu-id="62d32-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="62d32-255">Par conséquent, vous devrez peut-être aider ASP.NET en effectuant une conversion explicite.</span><span class="sxs-lookup"><span data-stu-id="62d32-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="62d32-256">Même si vous n’êtes pas obligé de convertir les types, il est parfois utile tester et pour voir quel type de données vous travaillez peut-être avec.</span><span class="sxs-lookup"><span data-stu-id="62d32-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="62d32-257">Le cas le plus courant est que vous devez convertir une chaîne en un autre type, comme un entier ou une date.</span><span class="sxs-lookup"><span data-stu-id="62d32-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="62d32-258">L’exemple suivant illustre un cas classique où vous devez convertir une chaîne en nombre.</span><span class="sxs-lookup"><span data-stu-id="62d32-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="62d32-259">En règle générale, l’entrée d’utilisateur vous parviennent sous forme de chaînes.</span><span class="sxs-lookup"><span data-stu-id="62d32-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="62d32-260">Même si vous avez invité de l’utilisateur à entrer un numéro, et même s’ils ont entré un chiffre, lorsque l’entrée d’utilisateur est soumise et lisez-le dans le code, les données sont au format de chaîne.</span><span class="sxs-lookup"><span data-stu-id="62d32-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="62d32-261">Par conséquent, vous devez convertir la chaîne en nombre.</span><span class="sxs-lookup"><span data-stu-id="62d32-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="62d32-262">Dans l’exemple, si vous essayez d’effectuer des opérations arithmétiques sur les valeurs sans les convertir, l’erreur suivante se produit, car ASP.NET ne peut pas ajouter deux chaînes :</span><span class="sxs-lookup"><span data-stu-id="62d32-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="62d32-263">Pour convertir les valeurs à des entiers, vous appelez le `AsInt` (méthode).</span><span class="sxs-lookup"><span data-stu-id="62d32-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="62d32-264">Si la conversion est réussie, vous pouvez ensuite ajouter les numéros.</span><span class="sxs-lookup"><span data-stu-id="62d32-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="62d32-265">Le tableau suivant répertorie les méthodes de conversion et de test habituellement pour les variables.</span><span class="sxs-lookup"><span data-stu-id="62d32-265">The following table lists some common conversion and test methods for variables.</span></span>


:::row:::
    :::column:::
        <strong>Method</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Example</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converts a string that represents a whole number (like &quot;593&quot;) to an integer.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number. (In ASP.NET, a decimal number is more precise than a floating-point number.)
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Converts a string that represents a date and time value to the ASP.NET `DateTime` type.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Converts any other data type to a string.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a><span data-ttu-id="62d32-266">Opérateurs</span><span class="sxs-lookup"><span data-stu-id="62d32-266">Operators</span></span>

<span data-ttu-id="62d32-267">Un opérateur est un mot clé ou un caractère qui indique à ASP.NET quel type de commande à effectuer dans une expression.</span><span class="sxs-lookup"><span data-stu-id="62d32-267">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="62d32-268">Visual Basic prend en charge de nombreux opérateurs, mais il vous suffit de reconnaître les quelques à commencer à développer des pages web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="62d32-268">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="62d32-269">Le tableau suivant récapitule les opérateurs courants.</span><span class="sxs-lookup"><span data-stu-id="62d32-269">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Examples</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Math operators used in numerical expressions.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assignment and equality. Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Inequality. Returns `True` if the values are not equal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Less than, greater than, less than or equal, and greater than or equal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concatenation, which is used to join strings.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        The increment and decrement operators, which add and subtract 1 (respectively) from a variable.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Dot. Used to distinguish objects and their properties and methods.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentheses. Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Not. Reverses a true value to false and vice versa. Typically used as a shorthand way to test for `False` (that is, for not `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        Logical AND and OR, which are used to link conditions together.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="62d32-270">Utilisation de fichiers et chemins d’accès de dossier dans le Code</span><span class="sxs-lookup"><span data-stu-id="62d32-270">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="62d32-271">Souvent, vous allez travailler avec les chemins d’accès de fichier et dossier dans votre code.</span><span class="sxs-lookup"><span data-stu-id="62d32-271">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="62d32-272">Voici un exemple de structure de dossiers physiques pour un site Web tel qu’il peut apparaître sur votre ordinateur de développement :</span><span class="sxs-lookup"><span data-stu-id="62d32-272">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="62d32-273">Voici quelques détails essentiels sur les URL et les chemins d’accès :</span><span class="sxs-lookup"><span data-stu-id="62d32-273">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="62d32-274">Une URL commence par un nom de domaine (`http://www.example.com`) ou un nom de serveur (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="62d32-274">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="62d32-275">Une URL correspond à un chemin d’accès physique sur un ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="62d32-275">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="62d32-276">Par exemple, `http://myserver` peuvent correspondre au dossier *C:\websites\mywebsite* sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="62d32-276">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="62d32-277">Un chemin d’accès virtuel est un raccourci pour représenter les chemins d’accès dans le code sans avoir à spécifier le chemin d’accès complet.</span><span class="sxs-lookup"><span data-stu-id="62d32-277">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="62d32-278">Il inclut la partie d’une URL qui suit le nom de domaine ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="62d32-278">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="62d32-279">Lorsque vous utilisez des chemins d’accès virtuels, vous pouvez déplacer votre code vers un autre domaine ou un serveur sans avoir à mettre à jour les chemins d’accès.</span><span class="sxs-lookup"><span data-stu-id="62d32-279">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="62d32-280">Voici un exemple pour vous aider à comprendre les différences :</span><span class="sxs-lookup"><span data-stu-id="62d32-280">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="62d32-281">URL complète</span><span class="sxs-lookup"><span data-stu-id="62d32-281">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="62d32-282">Nom du serveur</span><span class="sxs-lookup"><span data-stu-id="62d32-282">Server name</span></span> | *<span data-ttu-id="62d32-283">mycompanyserver</span><span class="sxs-lookup"><span data-stu-id="62d32-283">mycompanyserver</span></span>* |
| <span data-ttu-id="62d32-284">Chemin d'accès virtuel</span><span class="sxs-lookup"><span data-stu-id="62d32-284">Virtual path</span></span> | *<span data-ttu-id="62d32-285">/humanresources/CompanyPolicy.htm</span><span class="sxs-lookup"><span data-stu-id="62d32-285">/humanresources/CompanyPolicy.htm</span></span>* |
| <span data-ttu-id="62d32-286">Chemin d’accès physique</span><span class="sxs-lookup"><span data-stu-id="62d32-286">Physical path</span></span> | *<span data-ttu-id="62d32-287">C:\mywebsites\humanresources\CompanyPolicy.htm</span><span class="sxs-lookup"><span data-stu-id="62d32-287">C:\mywebsites\humanresources\CompanyPolicy.htm</span></span>* |

<span data-ttu-id="62d32-288">Est la racine virtuelle /, tout comme la racine de votre lecteur est \.</span><span class="sxs-lookup"><span data-stu-id="62d32-288">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="62d32-289">(Chemins d’accès du dossier virtuel utilisent toujours des barres obliques.) Le chemin d’accès virtuel d’un dossier ne doit pas avoir le même nom que le dossier physique ; Il peut être un alias.</span><span class="sxs-lookup"><span data-stu-id="62d32-289">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="62d32-290">(Sur les serveurs de production, le chemin d’accès virtuel rarement correspond à un chemin d’accès physique exact.)</span><span class="sxs-lookup"><span data-stu-id="62d32-290">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="62d32-291">Lorsque vous travaillez avec des fichiers et dossiers dans le code, parfois, vous devez référencer le chemin d’accès physique et parfois selon les objets que vous travaillez avec un chemin d’accès virtuel.</span><span class="sxs-lookup"><span data-stu-id="62d32-291">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="62d32-292">ASP.NET vous fournit ces outils pour l’utilisation des chemins d’accès de fichier et dossier dans le code : le `Server.MapPath` (méthode) et le `~` opérateur et `Href` (méthode).</span><span class="sxs-lookup"><span data-stu-id="62d32-292">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="62d32-293">Conversion des chemins d’accès virtuels physiques : la méthode Server.MapPath</span><span class="sxs-lookup"><span data-stu-id="62d32-293">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="62d32-294">Le `Server.MapPath` méthode convertit un chemin d’accès virtuel (comme */default.cshtml*) à un chemin d’accès physique absolu (comme *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="62d32-294">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="62d32-295">Vous utilisez cette méthode chaque fois que vous avez besoin d’un chemin d’accès physique complet.</span><span class="sxs-lookup"><span data-stu-id="62d32-295">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="62d32-296">Un exemple typique est lors de la lecture ou écriture d’un fichier texte ou un fichier image sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="62d32-296">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="62d32-297">En général, vous ne connaissez pas de chemin d’accès physique absolu de votre site sur le serveur d’hébergement d’un site, donc cette méthode peut convertir le chemin d’accès, vous connaissez — le chemin d’accès virtuel, le chemin d’accès correspondant sur le serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="62d32-297">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="62d32-298">Vous passez le chemin d’accès virtuel à un fichier ou dossier à la méthode et retourne le chemin d’accès physique :</span><span class="sxs-lookup"><span data-stu-id="62d32-298">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="62d32-299">Faisant référence à la racine virtuelle : le ~ opérateur et Href (méthode)</span><span class="sxs-lookup"><span data-stu-id="62d32-299">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="62d32-300">Dans un *.cshtml* ou *.vbhtml* fichier, vous pouvez référencer le chemin d’accès virtuel racine à l’aide du `~` opérateur.</span><span class="sxs-lookup"><span data-stu-id="62d32-300">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="62d32-301">Cela est très pratique, car vous pouvez déplacer les pages dans un site, et des liens qu’ils contiennent vers d’autres pages ne sera pas interrompues.</span><span class="sxs-lookup"><span data-stu-id="62d32-301">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="62d32-302">Il est également pratique au cas où vous décidez de déplacer votre site Web vers un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="62d32-302">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="62d32-303">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="62d32-303">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="62d32-304">Si le site Web est `http://myserver/myapp`, voici la façon dont ASP.NET traite ces chemins d’accès lors de l’exécution de la page :</span><span class="sxs-lookup"><span data-stu-id="62d32-304">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- `myImagesFolder`<span data-ttu-id="62d32-305">:</span><span class="sxs-lookup"><span data-stu-id="62d32-305">:</span></span> `http://myserver/myapp/images`
- `myStyleSheet` <span data-ttu-id="62d32-306">:</span><span class="sxs-lookup"><span data-stu-id="62d32-306">:</span></span> `http://myserver/myapp/styles/Stylesheet.css`

<span data-ttu-id="62d32-307">(Vous ne verrez pas ces chemins d’accès en tant que les valeurs de la variable, mais ASP.NET traite les chemins d’accès comme si c’est qu’ils étaient).</span><span class="sxs-lookup"><span data-stu-id="62d32-307">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="62d32-308">Vous pouvez utiliser le `~` opérateur à la fois dans le code serveur (comme ci-dessus) et dans le balisage, comme suit :</span><span class="sxs-lookup"><span data-stu-id="62d32-308">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="62d32-309">Dans le balisage, vous utilisez le `~` opérateur pour créer des chemins d’accès aux ressources telles que des fichiers image, d’autres pages web et de fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="62d32-309">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="62d32-310">Lorsque la page s’exécute, ASP.NET recherche via la page (de code et de balisage) et résout tous les `~` références pour le chemin d’accès approprié.</span><span class="sxs-lookup"><span data-stu-id="62d32-310">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="62d32-311">Boucles et la logique conditionnelle</span><span class="sxs-lookup"><span data-stu-id="62d32-311">Conditional Logic and Loops</span></span>

<span data-ttu-id="62d32-312">Code de serveur ASP.NET vous permet d’effectuer les tâches en fonction de conditions et d’écrire du code qui se répète les instructions de code, autrement dit, un certain nombre de fois qui s’exécute une boucle).</span><span class="sxs-lookup"><span data-stu-id="62d32-312">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="62d32-313">Conditions de tests</span><span class="sxs-lookup"><span data-stu-id="62d32-313">Testing conditions</span></span>

<span data-ttu-id="62d32-314">Pour tester une condition simple que vous utilisez le `If...Then` instruction, qui retourne `True` ou `False` basé sur un test que vous spécifiez :</span><span class="sxs-lookup"><span data-stu-id="62d32-314">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="62d32-315">Le `If` mot clé commence un bloc.</span><span class="sxs-lookup"><span data-stu-id="62d32-315">The `If` keyword starts a block.</span></span> <span data-ttu-id="62d32-316">Le test (condition) suit le `If` mot clé et retourne true ou false.</span><span class="sxs-lookup"><span data-stu-id="62d32-316">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="62d32-317">Le `If` instruction se termine par `Then`.</span><span class="sxs-lookup"><span data-stu-id="62d32-317">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="62d32-318">Les instructions qui s’exécutera si le test a la valeur true sont encadrées par des `If` et `End If`.</span><span class="sxs-lookup"><span data-stu-id="62d32-318">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="62d32-319">Un `If` instruction peut inclure un `Else` bloc qui spécifie les instructions à exécuter si la condition a la valeur false :</span><span class="sxs-lookup"><span data-stu-id="62d32-319">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="62d32-320">Si un `If` instruction commence un bloc de code, vous n’êtes pas obligé d’utiliser la normale `Code...End Code` instructions pour inclure les blocs.</span><span class="sxs-lookup"><span data-stu-id="62d32-320">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="62d32-321">Vous pouvez simplement ajouter `@` au bloc, et il ne fonctionnera pas.</span><span class="sxs-lookup"><span data-stu-id="62d32-321">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="62d32-322">Cette approche fonctionne avec `If` ainsi que d’autres mots clés qui sont suivis par les blocs de code, y compris de programmation de Visual Basic `For`, `For Each`, `Do While`, etc.</span><span class="sxs-lookup"><span data-stu-id="62d32-322">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="62d32-323">Vous pouvez ajouter plusieurs conditions à l’aide d’un ou plusieurs `ElseIf` blocs :</span><span class="sxs-lookup"><span data-stu-id="62d32-323">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="62d32-324">Dans cet exemple, si la première condition dans la `If` bloc n’est pas la valeur est true, le `ElseIf` condition est vérifiée.</span><span class="sxs-lookup"><span data-stu-id="62d32-324">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="62d32-325">Si cette condition est remplie, les instructions dans le `ElseIf` bloc sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="62d32-325">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="62d32-326">Si aucune des conditions sont remplies, les instructions dans le `Else` bloc sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="62d32-326">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="62d32-327">Vous pouvez ajouter un nombre quelconque de `ElseIf` bloque et puis fermer avec une `Else` bloquer comme le &quot;tout le reste&quot; condition.</span><span class="sxs-lookup"><span data-stu-id="62d32-327">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="62d32-328">Pour tester un grand nombre de conditions, utilisez un `Select Case` bloc :</span><span class="sxs-lookup"><span data-stu-id="62d32-328">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="62d32-329">La valeur à tester est entre parenthèses (dans l’exemple, la variable de jour de la semaine).</span><span class="sxs-lookup"><span data-stu-id="62d32-329">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="62d32-330">Chaque test individuel utilise un `Case` instruction qui indique une valeur.</span><span class="sxs-lookup"><span data-stu-id="62d32-330">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="62d32-331">Si la valeur d’un `Case` instruction correspond à la valeur de test, le code qui `Case` bloc est exécuté.</span><span class="sxs-lookup"><span data-stu-id="62d32-331">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="62d32-332">Le résultat des deux derniers blocs conditionnels affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-332">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="62d32-334">Code de boucle</span><span class="sxs-lookup"><span data-stu-id="62d32-334">Looping code</span></span>

<span data-ttu-id="62d32-335">Souvent, vous devez exécuter les mêmes instructions à plusieurs reprises.</span><span class="sxs-lookup"><span data-stu-id="62d32-335">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="62d32-336">Pour cela, en effectuant une boucle.</span><span class="sxs-lookup"><span data-stu-id="62d32-336">You do this by looping.</span></span> <span data-ttu-id="62d32-337">Par exemple, vous exécutez souvent les mêmes instructions pour chaque élément dans une collection de données.</span><span class="sxs-lookup"><span data-stu-id="62d32-337">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="62d32-338">Si vous savez exactement combien de fois que vous souhaitez effectuer une boucle, vous pouvez utiliser un `For` boucle.</span><span class="sxs-lookup"><span data-stu-id="62d32-338">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="62d32-339">Ce type de boucle est particulièrement utile pour allant ou le compte à rebours :</span><span class="sxs-lookup"><span data-stu-id="62d32-339">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="62d32-340">La boucle commence par la `For` mot clé, suivie de trois éléments :</span><span class="sxs-lookup"><span data-stu-id="62d32-340">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="62d32-341">Immédiatement après le `For` instruction, vous déclarez une variable de compteur (vous n’êtes pas obligé d’utiliser `Dim`), puis indiquez la plage, comme dans `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="62d32-341">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="62d32-342">Cela signifie que la variable `i` démarre à compter à 10 et continuer jusqu'à ce qu’il atteigne 20 (inclus).</span><span class="sxs-lookup"><span data-stu-id="62d32-342">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="62d32-343">Entre le `For` et `Next` instructions est le contenu du bloc.</span><span class="sxs-lookup"><span data-stu-id="62d32-343">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="62d32-344">Il peut contenir un ou plusieurs instructions de code qui s’exécutent avec chaque boucle.</span><span class="sxs-lookup"><span data-stu-id="62d32-344">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="62d32-345">La `Next i` instruction termine la boucle.</span><span class="sxs-lookup"><span data-stu-id="62d32-345">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="62d32-346">Il incrémente le compteur et démarre l’itération suivante de la boucle.</span><span class="sxs-lookup"><span data-stu-id="62d32-346">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="62d32-347">La ligne de code entre la `For` et `Next` lignes contient le code qui s’exécute pour chaque itération de la boucle.</span><span class="sxs-lookup"><span data-stu-id="62d32-347">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="62d32-348">Le balisage crée un nouveau paragraphe (`<p>` élément) chaque fois, ajoute une ligne à la sortie, en affichant la valeur de i (le compteur).</span><span class="sxs-lookup"><span data-stu-id="62d32-348">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="62d32-349">Lorsque vous exécutez cette page, l’exemple crée 11 lignes affichant la sortie, avec le texte dans chaque ligne indiquant le numéro d’élément.</span><span class="sxs-lookup"><span data-stu-id="62d32-349">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="62d32-351">Si vous travaillez avec une collection ou un tableau, vous utilisez souvent une `For Each` boucle.</span><span class="sxs-lookup"><span data-stu-id="62d32-351">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="62d32-352">Une collection est un groupe d’objets similaires et le `For Each` boucle vous permet de transporter une tâche sur chaque élément dans la collection.</span><span class="sxs-lookup"><span data-stu-id="62d32-352">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="62d32-353">Ce type de boucle est pratique pour les collections, car contrairement à un `For` boucle, vous n’êtes pas obligé incrément du compteur ou de définir une limite.</span><span class="sxs-lookup"><span data-stu-id="62d32-353">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="62d32-354">Au lieu de cela, le `For Each` code boucle passe simplement via la collection jusqu'à ce qu’elle est terminée.</span><span class="sxs-lookup"><span data-stu-id="62d32-354">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="62d32-355">Cet exemple retourne les éléments dans le `Request.ServerVariables` collection (qui contient des informations sur votre serveur web).</span><span class="sxs-lookup"><span data-stu-id="62d32-355">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="62d32-356">Il utilise un `For Each` boucle pour afficher le nom de chaque élément en créant un nouveau `<li>` élément dans une liste à puces HTML.</span><span class="sxs-lookup"><span data-stu-id="62d32-356">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="62d32-357">Le `For Each` mot clé est suivi par une variable qui représente un élément unique dans la collection (dans l’exemple, `myItem`), suivi par le `In` mot clé, suivie de la collection que vous souhaitez parcourir.</span><span class="sxs-lookup"><span data-stu-id="62d32-357">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="62d32-358">Dans le corps de la `For Each` boucle, vous pouvez accéder à l’élément actuel à l’aide de la variable que vous avez déclaré précédemment.</span><span class="sxs-lookup"><span data-stu-id="62d32-358">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="62d32-360">Pour créer une boucle plus généraliste, utilisez la `Do While` instruction :</span><span class="sxs-lookup"><span data-stu-id="62d32-360">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="62d32-361">Cette boucle commence par la `Do While` mot clé, suivie d’une condition, suivi par le bloc à répéter.</span><span class="sxs-lookup"><span data-stu-id="62d32-361">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="62d32-362">Incrémentent généralement des boucles (ajouter à) ou de décrémentation (Soustraire) une variable ou un objet utilisé pour le comptage.</span><span class="sxs-lookup"><span data-stu-id="62d32-362">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="62d32-363">Dans l’exemple, le `+=` opérateur ajoute 1 à la valeur d’une variable chaque fois que la boucle s’exécute.</span><span class="sxs-lookup"><span data-stu-id="62d32-363">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="62d32-364">(Pour décrémenter une variable dans une boucle qui compte à rebours, vous utiliseriez l’opérateur de décrémentation `-=`.)</span><span class="sxs-lookup"><span data-stu-id="62d32-364">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="62d32-365">Objets et Collections</span><span class="sxs-lookup"><span data-stu-id="62d32-365">Objects and Collections</span></span>

<span data-ttu-id="62d32-366">Presque tous les éléments dans un site Web ASP.NET sont un objet, y compris la page web elle-même.</span><span class="sxs-lookup"><span data-stu-id="62d32-366">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="62d32-367">Cette section décrit certains objets importants que vous utiliserez fréquemment dans votre code.</span><span class="sxs-lookup"><span data-stu-id="62d32-367">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="62d32-368">Objets de page</span><span class="sxs-lookup"><span data-stu-id="62d32-368">Page objects</span></span>

<span data-ttu-id="62d32-369">L’objet de base dans ASP.NET est la page.</span><span class="sxs-lookup"><span data-stu-id="62d32-369">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="62d32-370">Vous pouvez accéder à propriétés de l’objet page directement sans aucun objet éligible.</span><span class="sxs-lookup"><span data-stu-id="62d32-370">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="62d32-371">Le code suivant obtient le chemin d’accès du fichier de la page, à l’aide de la `Request` objet de la page :</span><span class="sxs-lookup"><span data-stu-id="62d32-371">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="62d32-372">Vous pouvez utiliser les propriétés de la `Page` objet pour obtenir un grand nombre d’informations, telles que :</span><span class="sxs-lookup"><span data-stu-id="62d32-372">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- `Request`<span data-ttu-id="62d32-373">.</span><span class="sxs-lookup"><span data-stu-id="62d32-373">.</span></span> <span data-ttu-id="62d32-374">Comme vous l’avez déjà vu, il s’agit d’une collection d’informations sur la requête actuelle, y compris le type de navigateur fait la demande, l’URL de la page, de l’identité de l’utilisateur, etc.</span><span class="sxs-lookup"><span data-stu-id="62d32-374">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- `Response`<span data-ttu-id="62d32-375">.</span><span class="sxs-lookup"><span data-stu-id="62d32-375">.</span></span> <span data-ttu-id="62d32-376">Il s’agit d’une collection d’informations sur la réponse (page) qui sera envoyée au navigateur une fois le code du serveur en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="62d32-376">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="62d32-377">Par exemple, vous pouvez utiliser cette propriété pour écrire des informations dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="62d32-377">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="62d32-378">Objets de collection (tableaux et les dictionnaires)</span><span class="sxs-lookup"><span data-stu-id="62d32-378">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="62d32-379">Une collection est un groupe d’objets du même type, telle qu’une collection de `Customer` objets à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="62d32-379">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="62d32-380">ASP.NET contient de nombreux regroupements intégrés, tels que le `Request.Files` collection.</span><span class="sxs-lookup"><span data-stu-id="62d32-380">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="62d32-381">Souvent, vous allez travailler avec des données dans des collections.</span><span class="sxs-lookup"><span data-stu-id="62d32-381">You'll often work with data in collections.</span></span> <span data-ttu-id="62d32-382">Deux types de collections courants sont le *tableau* et *dictionnaire*.</span><span class="sxs-lookup"><span data-stu-id="62d32-382">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="62d32-383">Un tableau est utile lorsque vous souhaitez stocker une collection d’éléments similaires mais ne souhaitez pas créer une variable distincte pour contenir chaque élément :</span><span class="sxs-lookup"><span data-stu-id="62d32-383">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="62d32-384">Les tableaux, vous déclarez un type de données spécifique, tel que `String`, `Integer`, ou `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="62d32-384">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="62d32-385">Pour indiquer que la variable peut contenir un tableau, vous ajoutez des parenthèses au nom de variable dans la déclaration (tel que `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="62d32-385">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="62d32-386">Vous pouvez accéder à des éléments dans un tableau à l’aide de leur position (index) ou en utilisant la `For Each` instruction.</span><span class="sxs-lookup"><span data-stu-id="62d32-386">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="62d32-387">Index de tableau sont de base zéro &#8212; autrement dit, le premier élément est en position 0, le deuxième élément est à la position 1 et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="62d32-387">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="62d32-388">Vous pouvez déterminer le nombre d’éléments dans un tableau en obtenant son `Length` propriété.</span><span class="sxs-lookup"><span data-stu-id="62d32-388">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="62d32-389">Pour obtenir la position d’un élément spécifique dans le tableau (autrement dit, pour rechercher le tableau), utilisez le `Array.IndexOf` (méthode).</span><span class="sxs-lookup"><span data-stu-id="62d32-389">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="62d32-390">Vous pouvez également effectuer les opérations comme inverse le contenu d’un tableau (la `Array.Reverse` méthode) ou trier le contenu (la `Array.Sort` méthode).</span><span class="sxs-lookup"><span data-stu-id="62d32-390">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="62d32-391">La sortie du code de tableau de chaîne affichée dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="62d32-391">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="62d32-393">Un dictionnaire est une collection de paires clé/valeur, où vous fournissez la clé (ou nom) pour définir ou récupérer la valeur correspondante :</span><span class="sxs-lookup"><span data-stu-id="62d32-393">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="62d32-394">Pour créer un dictionnaire, vous utilisez le `New` mot clé pour indiquer que vous créez un nouveau `Dictionary` objet.</span><span class="sxs-lookup"><span data-stu-id="62d32-394">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="62d32-395">Vous pouvez affecter un dictionnaire à une variable à l’aide du `Dim` mot clé.</span><span class="sxs-lookup"><span data-stu-id="62d32-395">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="62d32-396">Vous indiquez les types de données des éléments dans le dictionnaire à l’aide de parenthèses ( `( )` ).</span><span class="sxs-lookup"><span data-stu-id="62d32-396">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="62d32-397">À la fin de la déclaration, vous devez ajouter une autre paire de parenthèses, car il s’agit en fait une méthode qui crée un dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="62d32-397">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="62d32-398">Pour ajouter des éléments au dictionnaire, vous pouvez appeler la `Add` méthode de la variable de dictionnaire (`myScores` dans ce cas), puis spécifiez une clé et une valeur.</span><span class="sxs-lookup"><span data-stu-id="62d32-398">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="62d32-399">Ou bien, vous pouvez utiliser des parenthèses pour indiquer la clé et d’effectuer l’affectation simple, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="62d32-399">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="62d32-400">Pour obtenir une valeur à partir du dictionnaire, vous spécifiez la clé entre parenthèses :</span><span class="sxs-lookup"><span data-stu-id="62d32-400">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="62d32-401">Appel de méthodes avec des paramètres</span><span class="sxs-lookup"><span data-stu-id="62d32-401">Calling Methods with Parameters</span></span>

<span data-ttu-id="62d32-402">Comme vous l’avez vu précédemment dans cet article, les objets que vous programmez avec ont des méthodes.</span><span class="sxs-lookup"><span data-stu-id="62d32-402">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="62d32-403">Par exemple, un `Database` objet peut avoir un `Database.Connect` (méthode).</span><span class="sxs-lookup"><span data-stu-id="62d32-403">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="62d32-404">De nombreuses méthodes ont également un ou plusieurs paramètres.</span><span class="sxs-lookup"><span data-stu-id="62d32-404">Many methods also have one or more parameters.</span></span> <span data-ttu-id="62d32-405">Un *paramètre* est une valeur que vous passez à une méthode pour activer la méthode effectuer sa tâche.</span><span class="sxs-lookup"><span data-stu-id="62d32-405">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="62d32-406">Par exemple, examinez une déclaration pour le `Request.MapPath` (méthode), qui prend trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="62d32-406">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="62d32-407">Cette méthode retourne le chemin d’accès physique sur le serveur qui correspond à un chemin d’accès virtuel spécifié.</span><span class="sxs-lookup"><span data-stu-id="62d32-407">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="62d32-408">Les trois paramètres pour la méthode sont `virtualPath`, `baseVirtualDir`, et `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="62d32-408">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="62d32-409">(Notez que dans la déclaration, les paramètres sont répertoriés avec les types de données des données qu’ils accepte). Lorsque vous appelez cette méthode, vous devez fournir des valeurs pour tous les trois paramètres.</span><span class="sxs-lookup"><span data-stu-id="62d32-409">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="62d32-410">Lorsque vous utilisez Visual Basic avec la syntaxe Razor, vous avez deux options pour passer des paramètres à une méthode : *paramètres positionnels* ou *des paramètres nommés*.</span><span class="sxs-lookup"><span data-stu-id="62d32-410">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="62d32-411">Pour appeler une méthode à l’aide des paramètres positionnels, vous transmettez les paramètres dans un ordre strict est spécifié dans la déclaration de méthode.</span><span class="sxs-lookup"><span data-stu-id="62d32-411">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="62d32-412">(Vous généralement sauriez cet ordre en lisant la documentation de la méthode.) Vous devez suivre l’ordre, et vous ne pouvez pas ignorer les paramètres &#8212; si nécessaire, vous passez une chaîne vide (`""`) ou null pour que vous n’avez pas une valeur pour un paramètre de positionnement.</span><span class="sxs-lookup"><span data-stu-id="62d32-412">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="62d32-413">L’exemple suivant suppose que vous avez un dossier nommé *scripts* sur votre site Web.</span><span class="sxs-lookup"><span data-stu-id="62d32-413">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="62d32-414">Le code appelle la `Request.MapPath` et transmet des valeurs pour les trois paramètres dans l’ordre approprié.</span><span class="sxs-lookup"><span data-stu-id="62d32-414">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="62d32-415">Il affiche ensuite le chemin d’accès mappé qui en résulte.</span><span class="sxs-lookup"><span data-stu-id="62d32-415">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="62d32-416">Lorsqu’il existe de nombreux paramètres pour une méthode, vous pouvez conserver votre code plus propre et plus lisible à l’aide des paramètres nommés.</span><span class="sxs-lookup"><span data-stu-id="62d32-416">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="62d32-417">Pour appeler une méthode à l’aide de paramètres nommés, spécifiez le nom du paramètre suivi `:=` , puis indiquez la valeur.</span><span class="sxs-lookup"><span data-stu-id="62d32-417">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="62d32-418">L’avantage des paramètres nommés est que vous pouvez les ajouter dans l’ordre que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="62d32-418">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="62d32-419">(Un inconvénient est que l’appel de méthode n’est pas aussi compact.)</span><span class="sxs-lookup"><span data-stu-id="62d32-419">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="62d32-420">L’exemple suivant appelle la méthode décrite ci-dessus, mais utilise des paramètres nommés pour fournir les valeurs :</span><span class="sxs-lookup"><span data-stu-id="62d32-420">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="62d32-421">Comme vous pouvez le voir, les paramètres sont passés dans un ordre différent.</span><span class="sxs-lookup"><span data-stu-id="62d32-421">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="62d32-422">Toutefois, si vous exécutez l’exemple précédent et cet exemple, ils allons retourner la même valeur.</span><span class="sxs-lookup"><span data-stu-id="62d32-422">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="62d32-423">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="62d32-423">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="62d32-424">Instructions Try-Catch</span><span class="sxs-lookup"><span data-stu-id="62d32-424">Try-Catch statements</span></span>

<span data-ttu-id="62d32-425">Vous aurez souvent des instructions dans votre code peut échouer pour des raisons en dehors de votre contrôle.</span><span class="sxs-lookup"><span data-stu-id="62d32-425">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="62d32-426">Exemple :</span><span class="sxs-lookup"><span data-stu-id="62d32-426">For example:</span></span>

- <span data-ttu-id="62d32-427">Si votre code tente d’ouvrir, créer, lire ou écrire un fichier, que toutes sortes d’erreurs peuvent se produire.</span><span class="sxs-lookup"><span data-stu-id="62d32-427">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="62d32-428">Le fichier n’existe ne peut-être pas, il est peut-être verrouillé, le code ne peut pas disposer des autorisations et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="62d32-428">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="62d32-429">De même, si votre code essaie de mettre à jour des enregistrements dans une base de données, il peut y avoir des problèmes d’autorisations, la connexion à la base de données peut-être être supprimée, les données à enregistrer est peut-être non valide et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="62d32-429">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="62d32-430">En termes de programmation, ces situations sont appelées *exceptions*.</span><span class="sxs-lookup"><span data-stu-id="62d32-430">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="62d32-431">Si votre code rencontre une exception, il génère (lève) un message d’erreur qui est, au mieux, ennuyeux pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="62d32-431">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="62d32-433">Dans les situations où votre code peut rencontrer des exceptions et afin d’éviter les messages d’erreur de ce type, vous pouvez utiliser `Try/Catch` instructions.</span><span class="sxs-lookup"><span data-stu-id="62d32-433">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="62d32-434">Dans la `Try` instruction, vous exécutez le code que vous êtes en train de.</span><span class="sxs-lookup"><span data-stu-id="62d32-434">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="62d32-435">Dans un ou plusieurs `Catch` instructions, vous pouvez rechercher propres erreurs (types d’exceptions spécifiques) qui se sont produites.</span><span class="sxs-lookup"><span data-stu-id="62d32-435">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="62d32-436">Vous pouvez inclure autant `Catch` instructions que vous avez besoin rechercher des erreurs que vous êtes anticipation.</span><span class="sxs-lookup"><span data-stu-id="62d32-436">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="62d32-437">Nous vous recommandons d’éviter à l’aide de la `Response.Redirect` méthode dans `Try/Catch` instructions, car il peut provoquer une exception dans votre page.</span><span class="sxs-lookup"><span data-stu-id="62d32-437">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="62d32-438">L’exemple suivant montre une page qui crée un fichier texte à la première demande, puis affiche un bouton qui permet à l’utilisateur d’ouvrir le fichier.</span><span class="sxs-lookup"><span data-stu-id="62d32-438">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="62d32-439">L’exemple utilise délibérément un nom de fichier incorrect afin qu’elle entraîne une exception.</span><span class="sxs-lookup"><span data-stu-id="62d32-439">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="62d32-440">Le code inclut `Catch` instructions pour les deux exceptions possibles : `FileNotFoundException`, ce qui se produit si le nom de fichier est incorrect, et `DirectoryNotFoundException`, ce qui se produit si ASP.NET même Impossible de trouver le dossier.</span><span class="sxs-lookup"><span data-stu-id="62d32-440">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="62d32-441">(Vous pouvez ne pas commenter une instruction dans l’exemple pour voir comment elle s’exécute lorsque tout fonctionne correctement.)</span><span class="sxs-lookup"><span data-stu-id="62d32-441">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="62d32-442">Si votre code n’a pas gérer l’exception, vous voyez une page d’erreur comme la capture d’écran précédente.</span><span class="sxs-lookup"><span data-stu-id="62d32-442">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="62d32-443">Toutefois, le `Try/Catch` section vous aide à empêcher l’utilisateur de voir ces types d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="62d32-443">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="62d32-444">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="62d32-444">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="62d32-445">Documentation de référence</span><span class="sxs-lookup"><span data-stu-id="62d32-445">Reference Documentation</span></span>

- [<span data-ttu-id="62d32-446">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="62d32-446">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="62d32-447">Langage Visual Basic</span><span class="sxs-lookup"><span data-stu-id="62d32-447">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
