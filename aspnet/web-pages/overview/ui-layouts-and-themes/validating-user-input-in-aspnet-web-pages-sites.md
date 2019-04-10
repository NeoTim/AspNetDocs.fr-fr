---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validation des entrées d’utilisateur dans ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment valider des informations provenant d’utilisateurs &mdash; , autrement dit, s’assurer que les utilisateurs entrent valide les informations au format HTML forms dans un sous...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: fd3ba36891aa66f78c28c538a4d3ba0da6736765
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59392987"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="7721e-103">Validation des entrées d’utilisateur dans les Sites ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="7721e-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="7721e-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7721e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7721e-105">Cet article explique comment valider des informations provenant d’utilisateurs &mdash; , autrement dit, s’assurer que les utilisateurs entrent valide les informations au format HTML forms dans un site ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="7721e-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="7721e-106">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="7721e-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7721e-107">Comment vérifier qu’un entrée de l’utilisateur correspond aux critères de validation que vous définissez.</span><span class="sxs-lookup"><span data-stu-id="7721e-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="7721e-108">Comment déterminer si tous les tests de validation ont réussi.</span><span class="sxs-lookup"><span data-stu-id="7721e-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="7721e-109">Comment afficher les erreurs de validation (et comment les mettre en forme).</span><span class="sxs-lookup"><span data-stu-id="7721e-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="7721e-110">Comment valider les données qui ne proviennent directement les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="7721e-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="7721e-111">Il s’agit des concepts présentés dans l’article de programmation ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="7721e-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="7721e-112">Le `Validation` helper.</span><span class="sxs-lookup"><span data-stu-id="7721e-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="7721e-113">Le `Html.ValidationSummary` et `Html.ValidationMessage` méthodes.</span><span class="sxs-lookup"><span data-stu-id="7721e-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7721e-114">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="7721e-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7721e-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="7721e-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="7721e-116">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="7721e-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="7721e-117">Cet article contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="7721e-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="7721e-118">Vue d’ensemble de la Validation des entrées utilisateur</span><span class="sxs-lookup"><span data-stu-id="7721e-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="7721e-119">Validation des entrées utilisateur</span><span class="sxs-lookup"><span data-stu-id="7721e-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="7721e-120">Ajout d’une Validation côté Client</span><span class="sxs-lookup"><span data-stu-id="7721e-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="7721e-121">Mise en forme des erreurs de Validation</span><span class="sxs-lookup"><span data-stu-id="7721e-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="7721e-122">Validation des données qui ne proviennent directement d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="7721e-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="7721e-123">Vue d’ensemble de la Validation des entrées utilisateur</span><span class="sxs-lookup"><span data-stu-id="7721e-123">Overview of User Input Validation</span></span>

<span data-ttu-id="7721e-124">Si vous demandez aux utilisateurs d’entrer des informations dans une page, par exemple, dans un formulaire, il est important de s’assurer que les valeurs qui entrent sont valides.</span><span class="sxs-lookup"><span data-stu-id="7721e-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="7721e-125">Par exemple, vous ne souhaitez pas traiter un formulaire auquel il manque des informations critiques.</span><span class="sxs-lookup"><span data-stu-id="7721e-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="7721e-126">Quand les utilisateurs entrent des valeurs dans un formulaire HTML, ils entrent les valeurs sont des chaînes.</span><span class="sxs-lookup"><span data-stu-id="7721e-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="7721e-127">Dans de nombreux cas, les valeurs que vous avez besoin sont certains autres types de données, tels que des entiers ou des dates.</span><span class="sxs-lookup"><span data-stu-id="7721e-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="7721e-128">Par conséquent, vous devez également vous assurer que les valeurs que les utilisateurs entrent peuvent être convertis correctement aux types de données approprié.</span><span class="sxs-lookup"><span data-stu-id="7721e-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="7721e-129">Vous aurez peut-être également certaines restrictions sur les valeurs.</span><span class="sxs-lookup"><span data-stu-id="7721e-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="7721e-130">Même si les utilisateurs entrent correctement un entier, par exemple, vous devrez peut-être vous assurer que la valeur est comprise dans une certaine plage.</span><span class="sxs-lookup"><span data-stu-id="7721e-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Erreurs de validation qui utilisent des classes de style CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="7721e-132">**Important** validation des entrées utilisateur est également important pour la sécurité.</span><span class="sxs-lookup"><span data-stu-id="7721e-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="7721e-133">Lorsque vous limitez les valeurs que les utilisateurs peuvent entrer dans les formulaires, vous réduisez le risque que quelqu'un peut entrer une valeur qui peut compromettre la sécurité de votre site.</span><span class="sxs-lookup"><span data-stu-id="7721e-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="7721e-134">Validation des entrées utilisateur</span><span class="sxs-lookup"><span data-stu-id="7721e-134">Validating User Input</span></span>

<span data-ttu-id="7721e-135">Dans ASP.NET Web Pages 2, vous pouvez utiliser le `Validator` helper pour tester l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7721e-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="7721e-136">L’approche de base consiste à effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="7721e-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="7721e-137">Déterminer quels d’entrée des éléments (champs) que vous souhaitez valider.</span><span class="sxs-lookup"><span data-stu-id="7721e-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="7721e-138">Vous validez généralement les valeurs dans `<input>` éléments dans un formulaire.</span><span class="sxs-lookup"><span data-stu-id="7721e-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="7721e-139">Toutefois, il est conseillé de valider toutes les entrées, de même d’entrée qui provient d’un élément contraint comme un `<select>` liste.</span><span class="sxs-lookup"><span data-stu-id="7721e-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="7721e-140">Cela permet de s’assurer que les utilisateurs ne pas ignorer les contrôles sur une page et envoyer un formulaire.</span><span class="sxs-lookup"><span data-stu-id="7721e-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="7721e-141">Dans le code de la page, ajouter des contrôles de validation individuels pour chaque élément d’entrée à l’aide des méthodes de la `Validation` helper.</span><span class="sxs-lookup"><span data-stu-id="7721e-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="7721e-142">Pour vérifier les champs obligatoires, utilisez `Validation.RequireField(field, [error message])` (pour un champ individuel) ou `Validation.RequireFields(field1, field2, ...))` (pour obtenir la liste de champs).</span><span class="sxs-lookup"><span data-stu-id="7721e-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="7721e-143">Pour les autres types de validation, utilisez `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="7721e-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="7721e-144">Pour `ValidationType`, vous pouvez utiliser ces options :</span><span class="sxs-lookup"><span data-stu-id="7721e-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="7721e-145">Lorsque la page est envoyée, vérifiez si la validation a réussi en vérifiant `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="7721e-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="7721e-146">S’il existe des erreurs de validation, vous ignorez le traitement de la page normal.</span><span class="sxs-lookup"><span data-stu-id="7721e-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="7721e-147">Par exemple, si l’objectif de la page est mise à jour une base de données, vous ne le faites que jusqu'à ce que toutes les erreurs de validation ont été résolus.</span><span class="sxs-lookup"><span data-stu-id="7721e-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="7721e-148">S’il existe des erreurs de validation, afficher des messages d’erreur dans le balisage de la page à l’aide de `Html.ValidationSummary` ou `Html.ValidationMessage`, ou les deux.</span><span class="sxs-lookup"><span data-stu-id="7721e-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="7721e-149">L’exemple suivant montre une page qui illustre ces étapes.</span><span class="sxs-lookup"><span data-stu-id="7721e-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="7721e-150">Pour observer le fonctionne de la validation, exécutez cette page et délibérément commettent des erreurs.</span><span class="sxs-lookup"><span data-stu-id="7721e-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="7721e-151">Par exemple, voici comment la page se présente si vous oubliez d’entrer un nom de cours, si vous entrez un, et si vous entrez une date non valide :</span><span class="sxs-lookup"><span data-stu-id="7721e-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Erreurs de validation dans la page rendue](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="7721e-153">Ajout d’une Validation côté Client</span><span class="sxs-lookup"><span data-stu-id="7721e-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="7721e-154">Par défaut, l’entrée d’utilisateur est validée, une fois que les utilisateurs envoient la page, autrement dit, la validation est effectuée dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="7721e-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="7721e-155">L’inconvénient de cette approche est que les utilisateurs ne savent qu’ils avez apporté une erreur jusqu'à ce qu’après qu’ils envoient la page.</span><span class="sxs-lookup"><span data-stu-id="7721e-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="7721e-156">Si un formulaire est longue ou complexe, peut être gênant à l’utilisateur de rapports d’erreurs uniquement une fois que la page est envoyée.</span><span class="sxs-lookup"><span data-stu-id="7721e-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="7721e-157">Vous pouvez ajouter la prise en charge pour effectuer une validation dans le script client.</span><span class="sxs-lookup"><span data-stu-id="7721e-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="7721e-158">Dans ce cas, la validation est effectuée dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="7721e-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="7721e-159">Par exemple, supposons que vous spécifiez qu’une valeur doit être un entier.</span><span class="sxs-lookup"><span data-stu-id="7721e-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="7721e-160">Si un utilisateur entre une valeur non entière, l’erreur est signalée, dès que l’utilisateur quitte le champ d’entrée.</span><span class="sxs-lookup"><span data-stu-id="7721e-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="7721e-161">Les utilisateurs obtiennent des commentaires immédiats, qui est très utile pour eux.</span><span class="sxs-lookup"><span data-stu-id="7721e-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="7721e-162">Validation basée sur le client peut également réduire le nombre de fois où l’utilisateur doit envoyer le formulaire pour corriger plusieurs erreurs.</span><span class="sxs-lookup"><span data-stu-id="7721e-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="7721e-163">Même si vous utilisez la validation côté client, la validation est également toujours effectuée dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="7721e-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="7721e-164">La validation dans le code serveur d’est une mesure de sécurité, au cas où les utilisateurs contournent la validation basée sur le client.</span><span class="sxs-lookup"><span data-stu-id="7721e-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="7721e-165">Inscrire les bibliothèques JavaScript suivants dans la page :</span><span class="sxs-lookup"><span data-stu-id="7721e-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="7721e-166">Deux des bibliothèques sont chargés à partir d’un réseau de distribution de contenu (CDN), afin que vous ne sont pas nécessairement à les entrer sur votre ordinateur ou serveur.</span><span class="sxs-lookup"><span data-stu-id="7721e-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="7721e-167">Toutefois, vous devez disposer d’une copie locale de *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="7721e-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="7721e-168">Si vous ne travaillez pas déjà avec un modèle de WebMatrix (comme **Starter Site** ) qui inclut la bibliothèque, créez un site Web Pages qui repose sur **Starter Site**.</span><span class="sxs-lookup"><span data-stu-id="7721e-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="7721e-169">Copiez ensuite le *.js* fichier vers votre site actuel.</span><span class="sxs-lookup"><span data-stu-id="7721e-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="7721e-170">Dans le balisage, pour chaque élément que vous êtes validez, ajoutez un appel à `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="7721e-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="7721e-171">Cette méthode émet des attributs qui sont utilisés par la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="7721e-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="7721e-172">(Au lieu de l’émission de code JavaScript réel, la méthode émet des attributs tels que `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="7721e-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="7721e-173">Ces attributs prennent en charge la validation client non obstructifs qui utilise jQuery pour effectuer le travail.)</span><span class="sxs-lookup"><span data-stu-id="7721e-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="7721e-174">La page suivante montre comment ajouter des fonctionnalités de validation client pour l’exemple présentée.</span><span class="sxs-lookup"><span data-stu-id="7721e-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="7721e-175">Pas toutes les vérifications de validation exécutées sur le client.</span><span class="sxs-lookup"><span data-stu-id="7721e-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="7721e-176">En particulier, la validation de type de données (entier, date, etc.) ne s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="7721e-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="7721e-177">Les vérifications suivantes fonctionnent sur le client et le serveur :</span><span class="sxs-lookup"><span data-stu-id="7721e-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="7721e-178">Dans cet exemple, le test d’une date valide ne fonctionnera pas dans le code client.</span><span class="sxs-lookup"><span data-stu-id="7721e-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="7721e-179">Toutefois, le test sera exécuté dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="7721e-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="7721e-180">Mise en forme des erreurs de Validation</span><span class="sxs-lookup"><span data-stu-id="7721e-180">Formatting Validation Errors</span></span>

<span data-ttu-id="7721e-181">Vous pouvez contrôler l’affichent des erreurs de validation en définissant les classes CSS qui ont des noms réservés suivants :</span><span class="sxs-lookup"><span data-stu-id="7721e-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- `field-validation-error`<span data-ttu-id="7721e-182">.</span><span class="sxs-lookup"><span data-stu-id="7721e-182">.</span></span> <span data-ttu-id="7721e-183">Définit la sortie de la `Html.ValidationMessage` méthode lorsqu’il affiche une erreur.</span><span class="sxs-lookup"><span data-stu-id="7721e-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- `field-validation-valid`<span data-ttu-id="7721e-184">.</span><span class="sxs-lookup"><span data-stu-id="7721e-184">.</span></span> <span data-ttu-id="7721e-185">Définit la sortie de la `Html.ValidationMessage` méthode lorsqu’il n’existe aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="7721e-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- `input-validation-error`<span data-ttu-id="7721e-186">.</span><span class="sxs-lookup"><span data-stu-id="7721e-186">.</span></span> <span data-ttu-id="7721e-187">Définit comment `<input>` éléments sont rendus lorsqu’il existe une erreur.</span><span class="sxs-lookup"><span data-stu-id="7721e-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="7721e-188">(Par exemple, vous pouvez utiliser cette classe pour définir la couleur d’arrière-plan une &lt;d’entrée&gt; élément sur une couleur différente si sa valeur n’est pas valide.) Cette classe CSS est utilisée uniquement lors de la validation de client (dans ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="7721e-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- `input-validation-valid`<span data-ttu-id="7721e-189">.</span><span class="sxs-lookup"><span data-stu-id="7721e-189">.</span></span> <span data-ttu-id="7721e-190">Définit l’apparence des `<input>` éléments lorsqu’il n’existe aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="7721e-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- `validation-summary-errors`<span data-ttu-id="7721e-191">.</span><span class="sxs-lookup"><span data-stu-id="7721e-191">.</span></span> <span data-ttu-id="7721e-192">Définit la sortie de la `Html.ValidationSummary` méthode il affiche une liste d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="7721e-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- `validation-summary-valid`<span data-ttu-id="7721e-193">.</span><span class="sxs-lookup"><span data-stu-id="7721e-193">.</span></span> <span data-ttu-id="7721e-194">Définit la sortie de la `Html.ValidationSummary` méthode lorsqu’il n’existe aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="7721e-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="7721e-195">Ce qui suit `<style>` bloc montre les règles de conditions d’erreur.</span><span class="sxs-lookup"><span data-stu-id="7721e-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="7721e-196">Si vous incluez ce bloc de style dans l’exemple des pages précédemment dans cet article, l’affichage de l’erreur ressemblera à l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="7721e-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Erreurs de validation qui utilisent des classes de style CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="7721e-198">Si vous n’utilisez pas validation côté client dans ASP.NET Web Pages 2, les classes CSS pour le `<input>` éléments (`input-validation-error` et `input-validation-valid` n’a aucun effet.</span><span class="sxs-lookup"><span data-stu-id="7721e-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="7721e-199">Affichage des erreurs statiques et dynamiques</span><span class="sxs-lookup"><span data-stu-id="7721e-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="7721e-200">Les règles CSS sont fournis par paires, tels que `validation-summary-errors` et `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="7721e-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="7721e-201">Ces paires vous permettent de définir des règles pour les deux conditions : une condition d’erreur et une condition (non-error) « normale ».</span><span class="sxs-lookup"><span data-stu-id="7721e-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="7721e-202">Il est important de comprendre que le balisage pour l’affichage des erreurs est alors toujours restitué, même si aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="7721e-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="7721e-203">Par exemple, si une page a un `Html.ValidationSummary` méthode dans le balisage, la page source contient le balisage suivant même lorsque la page est demandée pour la première fois :</span><span class="sxs-lookup"><span data-stu-id="7721e-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="7721e-204">En d’autres termes, le `Html.ValidationSummary` méthode restitue toujours un `<div>` élément et une liste, même si la liste d’erreurs est vide.</span><span class="sxs-lookup"><span data-stu-id="7721e-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="7721e-205">De même, le `Html.ValidationMessage` méthode restitue toujours un `<span>` élément comme espace réservé pour une erreur de champ individuel, même si aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="7721e-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="7721e-206">Dans certaines situations, affichant un message d’erreur peuvent entraîner la page à la redistribution et peut entraîner des éléments dans la page à déplacer.</span><span class="sxs-lookup"><span data-stu-id="7721e-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="7721e-207">Les règles CSS qui se terminent par `-valid` vous permettent de définir une disposition qui peut aider à éviter ce problème.</span><span class="sxs-lookup"><span data-stu-id="7721e-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="7721e-208">Par exemple, vous pouvez définir `field-validation-error` et `field-validation-valid` pour les deux ont la même taille fixe.</span><span class="sxs-lookup"><span data-stu-id="7721e-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="7721e-209">De cette façon, la zone d’affichage pour le champ est statique et ne change pas le flux de page si un message d’erreur s’affiche.</span><span class="sxs-lookup"><span data-stu-id="7721e-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="7721e-210">Validation des données qui ne proviennent directement d’utilisateurs</span><span class="sxs-lookup"><span data-stu-id="7721e-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="7721e-211">Parfois, vous devez valider les informations qui ne proviennent directement d’un formulaire HTML.</span><span class="sxs-lookup"><span data-stu-id="7721e-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="7721e-212">Un exemple classique est une page où une valeur est passée dans une chaîne de requête, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="7721e-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="7721e-213">Dans ce cas, vous souhaitez vous assurer que la valeur est passée à la page (ici, 1022 pour la valeur de `classid`) est valide.</span><span class="sxs-lookup"><span data-stu-id="7721e-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="7721e-214">Vous ne pouvez pas utiliser directement le `Validation` helper pour effectuer cette validation.</span><span class="sxs-lookup"><span data-stu-id="7721e-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="7721e-215">Toutefois, vous pouvez utiliser d’autres fonctionnalités du système de contrôle, comme la possibilité d’afficher des messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="7721e-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7721e-216">**Important** toujours valider les valeurs que vous obtenez à partir de *n’importe quel* source, y compris les valeurs de champ de formulaire, les valeurs de chaîne de requête et valeurs de cookie.</span><span class="sxs-lookup"><span data-stu-id="7721e-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="7721e-217">Il est facile pour les personnes à modifier ces valeurs (par exemple à des fins malveillantes).</span><span class="sxs-lookup"><span data-stu-id="7721e-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="7721e-218">Par conséquent, vous devez vérifier ces valeurs afin de protéger votre application.</span><span class="sxs-lookup"><span data-stu-id="7721e-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="7721e-219">L’exemple suivant montre comment vous pouvez valider une valeur qui est passée dans une chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="7721e-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="7721e-220">Le code vérifie que la valeur n’est pas vide et qu’il s’agit d’un entier.</span><span class="sxs-lookup"><span data-stu-id="7721e-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="7721e-221">Notez que le test est effectué lorsque la demande n’est pas une soumission de formulaire (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="7721e-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="7721e-222">Ce test peut passer la première fois que la page est demandée, mais pas lorsque la demande est une soumission de formulaire.</span><span class="sxs-lookup"><span data-stu-id="7721e-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="7721e-223">Pour afficher cette erreur, vous pouvez ajouter l’erreur à la liste des erreurs de validation en appelant `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="7721e-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="7721e-224">Si la page contient un appel à la `Html.ValidationSummary` (méthode), l’erreur s’affiche, comme une erreur de validation d’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7721e-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7721e-225">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7721e-225">Additional Resources</span></span>

[<span data-ttu-id="7721e-226">Utilisation des formulaires HTML dans les Sites ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="7721e-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
