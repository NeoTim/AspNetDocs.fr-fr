---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: >
  Créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation de mot de passe (C#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 avec confirmation par courrier électronique et réinitialisation de mot de passe en utilisant le système d’appartenance d'ASP.NET Identity. Autorité de certification vous...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 5092476c6cf59bea6fab6fa6f169ff11ec4c9c4a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030276"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="10c96-104">Créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par courrier électronique et réinitialisation de mot de passe (C#)
</span><span class="sxs-lookup"><span data-stu-id="10c96-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="10c96-105">par [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="10c96-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="10c96-106">Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 avec confirmation par courrier électronique et réinitialisation de mot de passe en utilisant le système d’appartenance d'ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="10c96-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="10c96-107">Vous pouvez télécharger l’application terminée [ici](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="10c96-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="10c96-108">Le téléchargement contient les helpers de débogage qui vous permettent de tester une confirmation par e-mail et par SMS sans configurer un fournisseur de messagerie ou de SMS.</span><span class="sxs-lookup"><span data-stu-id="10c96-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="10c96-109">Ce didacticiel a été écrit par [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter : [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="10c96-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="10c96-110">Créer une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="10c96-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="10c96-111">Démarrez en installant et en cours d’exécution [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="10c96-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="10c96-112">Installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="10c96-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="10c96-113">Avertissement : Vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure pour suivre ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="10c96-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="10c96-114">Créez un projet Web ASP.NET et sélectionnez le modèle MVC.</span><span class="sxs-lookup"><span data-stu-id="10c96-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="10c96-115">Web Forms prend également en charge ASP.NET Identity : vous pouvez donc suivre des étapes similaires dans une application Web Forms.</span><span class="sxs-lookup"><span data-stu-id="10c96-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="10c96-116">Laissez l’authentification par défaut à **Comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="10c96-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="10c96-117">Si vous souhaitez héberger l’application dans Azure, laissez la case à cocher activée.</span><span class="sxs-lookup"><span data-stu-id="10c96-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="10c96-118">Plus loin dans ce didacticiel, nous déploierons sur Azure.</span><span class="sxs-lookup"><span data-stu-id="10c96-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="10c96-119">Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="10c96-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="10c96-120">Définir le [projet pour utiliser SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="10c96-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="10c96-121">Exécutez l’application, cliquez sur le lien **S'inscrire** et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="10c96-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="10c96-122">À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="10c96-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="10c96-123">Dans l’Explorateur de serveurs, accédez à **Data Connections\DefaultConnection\Tables\AspNetUsers**, cliquez avec le bouton droit et sélectionnez **Ouvrir la définition de la table**.</span><span class="sxs-lookup"><span data-stu-id="10c96-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="10c96-124">L’illustration suivante montre le schéma `AspNetUsers` :</span><span class="sxs-lookup"><span data-stu-id="10c96-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="10c96-125">Cliquez avec le bouton droit sur la table **AspNetUsers** et sélectionnez **Afficher les données de la table**.</span><span class="sxs-lookup"><span data-stu-id="10c96-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="10c96-126">À ce stade, l’adresse de messagerie n’a pas été confirmée.
</span><span class="sxs-lookup"><span data-stu-id="10c96-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="10c96-127">Cliquez sur la ligne, puis sélectionnez Supprimer.</span><span class="sxs-lookup"><span data-stu-id="10c96-127">Click on the row and select delete.</span></span> <span data-ttu-id="10c96-128">Vous ajoutez de nouveau ce courrier électronique à l’étape suivante et envoyer un e-mail de confirmation.</span><span class="sxs-lookup"><span data-stu-id="10c96-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="10c96-129">E-mail de confirmation</span><span class="sxs-lookup"><span data-stu-id="10c96-129">Email confirmation</span></span>

<span data-ttu-id="10c96-130">Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur pour vérifier qu’ils n’empruntent pas l'identité d'une autre personne (autrement dit, qu'ils ne se sont pas inscrits avec par l'adresse de messagerie de quelqu'un d’autre).</span><span class="sxs-lookup"><span data-stu-id="10c96-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="10c96-131">Supposons que vous ayez un forum de discussion : vous voudriez empêcher `"bob@example.com"` de s'inscrire en tant que `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="10c96-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="10c96-132">ans la confirmation par courrier électronique, `"joe@contoso.com"` pourrait recevoir du courrier indésirable à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="10c96-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="10c96-133"> Supposons que Bob se soit accidentellement inscrit en tant que `"bib@example.com"` et ne l'ait pas remarqué, il ne serait pas en mesure d’utiliser le mot de passe de récupération, car l’application n’.</span><span class="sxs-lookup"><span data-stu-id="10c96-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="10c96-134">E-mail de confirmation fournit uniquement une protection limitée des robots et ne fournit pas une protection à partir des expéditeurs de courrier indésirable déterminés, ils ont des alias de messagerie travail nombreux, qu'ils peuvent utiliser pour inscrire.</span><span class="sxs-lookup"><span data-stu-id="10c96-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="10c96-135">En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web avant qu’ils soient confirmés par courrier électronique, par SMS ou via un autre mécanisme.</span><span class="sxs-lookup"><span data-stu-id="10c96-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="10c96-136">Dans les sections ci-dessous, nous allons activer la confirmation de courrier électronique et modifier le code pour empêcher les utilisateurs qui viennent d’être inscrits de se connecter jusqu'à ce que leur courrier électronique ait été confirmé.</span><span class="sxs-lookup"><span data-stu-id="10c96-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="10c96-137">Se connecter à SendGrid</span><span class="sxs-lookup"><span data-stu-id="10c96-137">Hook up SendGrid</span></span>

<span data-ttu-id="10c96-138">Les instructions fournies dans cette section ne sont pas en cours.</span><span class="sxs-lookup"><span data-stu-id="10c96-138">The instructions in this section are not current.</span></span> <span data-ttu-id="10c96-139">Consultez [fournisseur de messagerie SendGrid configurer](/aspnet/core/security/authentication/accconfirm#configure-email-provider) pour les instructions de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="10c96-139">See [Configure SendGrid email provider](/aspnet/core/security/authentication/accconfirm#configure-email-provider) for updated instructions.</span></span>

<span data-ttu-id="10c96-140">Bien que ce didacticiel montre uniquement comment ajouter la notification par courrier électronique via [SendGrid](http://sendgrid.com/), vous pouvez envoyer un e-mail à l’aide de SMTP et d'autres mécanismes (consultez [les ressources additionnelles](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="10c96-140">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="10c96-141">Dans la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="10c96-141">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="10c96-142">Accédez à la [page d’inscription d'Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) et inscrivez-vous à un compte SendGrid gratuit.</span><span class="sxs-lookup"><span data-stu-id="10c96-142">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="10c96-143">Configurez SendGrid en ajoutant du code semblable au suivant dans *App_Start/IdentityConfig.cs* :</span><span class="sxs-lookup"><span data-stu-id="10c96-143">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="10c96-144">Vous devrez ajouter les inclusions suivantes :</span><span class="sxs-lookup"><span data-stu-id="10c96-144">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="10c96-145">Pour simplifier cet exemple, nous allons stocker les paramètres d’application dans le fichier *web.config* :</span><span class="sxs-lookup"><span data-stu-id="10c96-145">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="10c96-146">Sécurité - Ne jamais stocker de données sensibles dans votre code source.</span><span class="sxs-lookup"><span data-stu-id="10c96-146">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="10c96-147">Le compte et les informations d’identification sont stockés dans l’appSetting.</span><span class="sxs-lookup"><span data-stu-id="10c96-147">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="10c96-148"> Sur Azure, vous pouvez stocker en toute sécurité ces valeurs sur l'onglet **[Configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="10c96-148">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="10c96-149">Consultez [les meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="10c96-149">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="10c96-150">Activer la confirmation par courrier électronique dans le contrôleur de compte</span><span class="sxs-lookup"><span data-stu-id="10c96-150">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="10c96-151">Vérifiez que le fichier *Views\Account\ConfirmEmail.cshtml* a une syntaxe razor correcte.</span><span class="sxs-lookup"><span data-stu-id="10c96-151">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="10c96-152">(Le @ caractère dans la première ligne est peut-être manquant.</span><span class="sxs-lookup"><span data-stu-id="10c96-152">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="10c96-153">)</span><span class="sxs-lookup"><span data-stu-id="10c96-153">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="10c96-154">Exécutez l’application et cliquez sur le lien S’inscrire. </span><span class="sxs-lookup"><span data-stu-id="10c96-154">Run the app and click the Register link.</span></span> <span data-ttu-id="10c96-155">Une fois que vous avez envoyé le formulaire d’inscription, vous êtes connecté.</span><span class="sxs-lookup"><span data-stu-id="10c96-155">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="10c96-156">Vérifiez votre compte de messagerie, puis cliquez sur le lien pour confirmer votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="10c96-156">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="10c96-157">Demander une confirmation par courrier électronique avant la connexion</span><span class="sxs-lookup"><span data-stu-id="10c96-157">Require email confirmation before log in</span></span>

<span data-ttu-id="10c96-158">Actuellement, une fois qu’un utilisateur complète le formulaire d’inscription, il est connecté.</span><span class="sxs-lookup"><span data-stu-id="10c96-158">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="10c96-159">En règle générale, vous voulez vérifier son adresse e-mail avant de le connecter.</span><span class="sxs-lookup"><span data-stu-id="10c96-159">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="10c96-160">Dans la section ci-dessous, nous modifions le code pour demander aux nouveaux utilisateurs un e-mail vérifié avant de les connecter (les authentifier).</span><span class="sxs-lookup"><span data-stu-id="10c96-160">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="10c96-161">Mettez à jour la méthode `HttpPost Register` avec les modifications en surbrillance suivantes :</span><span class="sxs-lookup"><span data-stu-id="10c96-161">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="10c96-162">Si vous décommentez la méthode `SignInAsync`, l’utilisateur ne sera pas connecté lors de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="10c96-162">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="10c96-163">La ligne `TempData["ViewBagLink"] = callbackUrl;` peut être utilisée pour [déboguer l’application](#dbg) et tester l’inscription sans envoyer de courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="10c96-163">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="10c96-164">`ViewBag.Message` permet d’afficher les instructions de confirmation.</span><span class="sxs-lookup"><span data-stu-id="10c96-164">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="10c96-165">L'[exemple à télécharger](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contient du code pour tester l’e-mail de confirmation sans configurer la messagerie et peut également être utilisé pour déboguer l’application.</span><span class="sxs-lookup"><span data-stu-id="10c96-165">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="10c96-166">Créez un fichier `Views\Shared\Info.cshtml` et ajoutez le balisage razor suivant :</span><span class="sxs-lookup"><span data-stu-id="10c96-166">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="10c96-167">Ajoutez l' [attribut Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) à la méthode d’action `Contact` du contrôleur Home.</span><span class="sxs-lookup"><span data-stu-id="10c96-167">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="10c96-168">Vous pouvez cliquer sur le **Contact** lien pour vérifier les utilisateurs anonymes n’ont accès et les utilisateurs authentifiés ont accès.</span><span class="sxs-lookup"><span data-stu-id="10c96-168">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="10c96-169">Vous devez également mettre à jour la méthode d’action `HttpPost Login`:</span><span class="sxs-lookup"><span data-stu-id="10c96-169">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="10c96-170">Mettez à jour la vue *Views\Shared\Error.cshtml* pour afficher le message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="10c96-170">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="10c96-171">Supprimez les comptes de la table **AspNetUsers** qui contiennent l’alias d’e-mail que vous voulez tester.</span><span class="sxs-lookup"><span data-stu-id="10c96-171">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="10c96-172">Exécutez l’application et vérifiez que vous ne pouvez pas vous connecter avant d’avoir confirmé votre adresse e-mail.</span><span class="sxs-lookup"><span data-stu-id="10c96-172">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="10c96-173">Après avoir confirmé votre adresse e-mail, cliquez sur le lien **Contact**.</span><span class="sxs-lookup"><span data-stu-id="10c96-173">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="10c96-174">Récupération de mot de passe/réinitialisation</span><span class="sxs-lookup"><span data-stu-id="10c96-174">Password recovery/reset</span></span>

<span data-ttu-id="10c96-175">Supprimez les caractères de commentaire de la `HttpPost ForgotPassword` méthode d’action dans le contrôleur de compte :</span><span class="sxs-lookup"><span data-stu-id="10c96-175">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="10c96-176">Supprimez les caractères de commentaire de la méthode `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) dans le fichier de vue razor *Views\Account\Login.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="10c96-176">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="10c96-177">La page de connexion affiche maintenant un lien pour réinitialiser le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="10c96-177">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="10c96-178">Renvoyer le lien de confirmation de courrier électronique</span><span class="sxs-lookup"><span data-stu-id="10c96-178">Resend email confirmation link</span></span>

<span data-ttu-id="10c96-179">Une fois qu’un utilisateur crée un compte local, un lien de confirmation lui est envoyé par e-mail, qu’il doit utiliser avant de pouvoir se connecter.</span><span class="sxs-lookup"><span data-stu-id="10c96-179">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="10c96-180">Si l’utilisateur supprime accidentellement l’e-mail de confirmation ou si l’e-mail n’arrive jamais, le lien de confirmation doit lui être renvoyé.</span><span class="sxs-lookup"><span data-stu-id="10c96-180">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="10c96-181">Les modifications suivantes du code montrent comment faire cela.
</span><span class="sxs-lookup"><span data-stu-id="10c96-181">The following code changes show how to enable this.</span></span>

<span data-ttu-id="10c96-182">Ajoutez la méthode helper suivante en bas du fichier *Controllers\AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="10c96-182">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="10c96-183">Mettez à jour la méthode Register de façon à utiliser le nouveau helper :</span><span class="sxs-lookup"><span data-stu-id="10c96-183">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="10c96-184">Mettez à jour la méthode Login de façon à renvoyer le mot de passe si le compte d’utilisateur n’a pas été confirmé :</span><span class="sxs-lookup"><span data-stu-id="10c96-184">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="10c96-185">Combiner des comptes de connexion de réseaux sociaux et local</span><span class="sxs-lookup"><span data-stu-id="10c96-185">Combine social and local login accounts</span></span>

<span data-ttu-id="10c96-186">Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="10c96-186">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="10c96-187">Dans la séquence suivante, **RickAndMSFT@gmail.com** est d’abord créé en tant que connexion locale, mais vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="10c96-187">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="10c96-188">Cliquez sur le lien **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="10c96-188">Click on the **Manage** link.</span></span> <span data-ttu-id="10c96-189">Remarque la **des connexions externes : 0** associé à ce compte.</span><span class="sxs-lookup"><span data-stu-id="10c96-189">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="10c96-190">Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="10c96-190">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="10c96-191">Les deux comptes ont été combinés : vous pourrez donc vous connecter avec l'un ou l'autre compte.</span><span class="sxs-lookup"><span data-stu-id="10c96-191">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="10c96-192">Vous pouvez permettre à vos utilisateurs d’ajouter des comptes locaux au cas où leur service d’authentification de connexion de réseau social serait indisponible, ou au cas plus probable où ils auraient perdu l’accès à leur compte social.</span><span class="sxs-lookup"><span data-stu-id="10c96-192">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="10c96-193">Dans l’image suivante, Tom est le journal dans un réseau social (que vous pouvez voir à partir de la **des connexions externes : 1** indiqué sur la page).</span><span class="sxs-lookup"><span data-stu-id="10c96-193">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="10c96-194">Le fait de cliquer sur **Choisir un mot de passe** vous permet d’ajouter une connexion locale associée au même compte.</span><span class="sxs-lookup"><span data-stu-id="10c96-194">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="10c96-195">E-mail de confirmation de façon plus approfondie</span><span class="sxs-lookup"><span data-stu-id="10c96-195">Email confirmation in more depth</span></span>

<span data-ttu-id="10c96-196">Mon didacticiel [Confirmation du compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) aborde ce sujet avec plus de détails.</span><span class="sxs-lookup"><span data-stu-id="10c96-196">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="10c96-197">Débogage de l’application</span><span class="sxs-lookup"><span data-stu-id="10c96-197">Debugging the app</span></span>

<span data-ttu-id="10c96-198">Si vous n’obtenez pas un message électronique contenant le lien :</span><span class="sxs-lookup"><span data-stu-id="10c96-198">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="10c96-199">Vérifiez votre dossier des courriers indésirables.</span><span class="sxs-lookup"><span data-stu-id="10c96-199">Check your junk or spam folder.</span></span>
- <span data-ttu-id="10c96-200">Connectez-vous à votre compte SendGrid, puis cliquez sur le [lien activité de messagerie](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="10c96-200">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="10c96-201">Pour tester le lien de vérification sans e-mail, vous devez télécharger l'[exemple terminé](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="10c96-201">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="10c96-202">Le lien de confirmation et les codes de confirmation seront affichés dans la page.</span><span class="sxs-lookup"><span data-stu-id="10c96-202">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="10c96-203">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="10c96-203">Additional Resources</span></span>

- [<span data-ttu-id="10c96-204">Des liens vers les ressources recommandées sur ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="10c96-204">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="10c96-205">[Confirmation de compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Décrit plus en détails la confirmation de compte et la récupération de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="10c96-205">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="10c96-206">[Application MVC 5 avec connexion Facebook, Twitter, LinkedIn et Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ce tutoriel vous montre comment écrire une application ASP.NET MVC 5 avec une autorisation Facebook et Google OAuth2.</span><span class="sxs-lookup"><span data-stu-id="10c96-206">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="10c96-207">Il montre également comment ajouter des données supplémentaires à la base de données Identity.</span><span class="sxs-lookup"><span data-stu-id="10c96-207">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="10c96-208">[Déployer une application sécurisée ASP.NET MVC avec l’appartenance, OAuth et SQL Database sur Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="10c96-208">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="10c96-209">Ce tutoriel ajoute le déploiement Azure, la sécurisation de votre application avec des rôles, l’utilisation de l’API d’appartenance pour ajouter des utilisateurs et des rôles, et des fonctionnalités de sécurité supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="10c96-209">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="10c96-210">Création d’une application pour Google OAuth 2 et connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="10c96-210">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="10c96-211">Création de l’application dans Facebook et connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="10c96-211">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="10c96-212">Configuration de SSL dans le projet</span><span class="sxs-lookup"><span data-stu-id="10c96-212">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
