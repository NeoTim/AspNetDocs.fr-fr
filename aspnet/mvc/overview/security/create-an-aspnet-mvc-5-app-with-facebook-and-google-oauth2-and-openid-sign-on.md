---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Créez l’application MVC 5 avec Facebook, Twitter, LinkedIn et Google OAuth2 Sign-on (C) Microsoft Docs
author: Rick-Anderson
description: Ce tutoriel vous montre comment construire une application web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2.0 avec des informations d’identification d’un authenti externe...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676320"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="aed8d-103">Créer une application ASP.NET MVC 5 avec authentification OAuth2 Facebook, Twitter, LinkedIn et Google (C#)</span><span class="sxs-lookup"><span data-stu-id="aed8d-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>

<span data-ttu-id="aed8d-104">par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aed8d-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="aed8d-105">Ce tutoriel vous montre comment construire une application web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide [d’OAuth 2.0](http://oauth.net/2/) avec des informations d’identification d’un fournisseur d’authentification externe, tels que Facebook, Twitter, LinkedIn, Microsoft ou Google.</span><span class="sxs-lookup"><span data-stu-id="aed8d-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="aed8d-106">Pour plus de simplicité, ce tutoriel se concentre sur le travail avec les informations d’identification de Facebook et Google.</span><span class="sxs-lookup"><span data-stu-id="aed8d-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="aed8d-107">L’activation de ces informations d’identification sur vos sites Web offre un avantage significatif car des millions d’utilisateurs ont déjà des comptes auprès de ces fournisseurs externes.</span><span class="sxs-lookup"><span data-stu-id="aed8d-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="aed8d-108">Ces utilisateurs peuvent être plus enclins à s’inscrire à votre site s’ils n’ont pas à créer et à se souvenir d’un nouvel ensemble d’informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="aed8d-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="aed8d-109">Voir aussi [ASP.NET application MVC 5 avec SMS et e-mail Authentification à deux facteurs](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="aed8d-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="aed8d-110">Le tutoriel montre également comment ajouter des données de profil pour l’utilisateur, et comment utiliser l’API d’adhésion pour ajouter des rôles.</span><span class="sxs-lookup"><span data-stu-id="aed8d-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="aed8d-111">Ce tutoriel a été écrit par [Rick](https://blogs.msdn.com/rickAndy) [@RickAndMSFT](https://twitter.com/RickAndMSFT) Anderson ( S’il vous plaît suivez-moi sur Twitter: ).</span><span class="sxs-lookup"><span data-stu-id="aed8d-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="aed8d-112">Mise en route</span><span class="sxs-lookup"><span data-stu-id="aed8d-112">Getting Started</span></span>

<span data-ttu-id="aed8d-113">Commencez par installer et exécuter [Visual Studio Express 2013 pour Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou Visual Studio [2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="aed8d-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="aed8d-114">Installez Visual Studio [2013 Mise à jour 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou plus.</span><span class="sxs-lookup"><span data-stu-id="aed8d-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="aed8d-115">Pour de l’aide avec Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, et plus encore, voir ce [projet d’exemple](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="aed8d-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="aed8d-116">Vous devez installer Visual Studio [2013 Mise à jour 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou plus pour utiliser Google OAuth 2 et de débouger localement sans avertissements SSL.</span><span class="sxs-lookup"><span data-stu-id="aed8d-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>

<span data-ttu-id="aed8d-117">Cliquez sur **New Project** à partir de la page **Démarrer,** ou vous pouvez utiliser le menu et sélectionner **le fichier**, puis new **Project**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="aed8d-118">Création de votre première application</span><span class="sxs-lookup"><span data-stu-id="aed8d-118">Creating Your First Application</span></span>

<span data-ttu-id="aed8d-119">Cliquez sur **New Project**, puis sélectionnez Visual **C sur** la gauche, puis sur le **Web,** puis sélectionnez **ASP.NET application Web**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="aed8d-120">Nommez votre projet "MvcAuth" et cliquez **ensuite sur OK**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="aed8d-121">Dans le dialogue **du projet New ASP.NET,** cliquez sur **MVC**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="aed8d-122">Si l’authentification **n’est**pas un compte utilisateur individuel, cliquez sur le bouton **Modifier l’authentification** et sélectionnez **les comptes utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="aed8d-123">En vérifiant **Host dans le cloud**, l’application sera très facile à accueillir en Azure.</span><span class="sxs-lookup"><span data-stu-id="aed8d-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="aed8d-124">Si vous avez sélectionné **Host dans le cloud,** complétez le dialogue configuré.</span><span class="sxs-lookup"><span data-stu-id="aed8d-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="aed8d-125">Utilisez NuGet pour mettre à jour le dernier middleware OWIN</span><span class="sxs-lookup"><span data-stu-id="aed8d-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="aed8d-126">Utilisez le gestionnaire de paquets NuGet pour mettre à jour le [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="aed8d-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="aed8d-127">Sélectionnez **les mises à jour** dans le menu gauche.</span><span class="sxs-lookup"><span data-stu-id="aed8d-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="aed8d-128">Vous pouvez cliquer sur le bouton **Mise à jour Tous** ou vous pouvez rechercher uniquement des paquets OWIN (indiqués dans l’image suivante) :</span><span class="sxs-lookup"><span data-stu-id="aed8d-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="aed8d-129">Dans l’image ci-dessous, seuls les paquets OWIN sont affichés :</span><span class="sxs-lookup"><span data-stu-id="aed8d-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="aed8d-130">Depuis la console Package Manager (PMC), vous pouvez entrer la `Update-Package` commande, qui mettra à jour tous les paquets.</span><span class="sxs-lookup"><span data-stu-id="aed8d-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="aed8d-131">Appuyez sur **F5** ou **Ctrl-F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="aed8d-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="aed8d-132">Dans l’image ci-dessous, le numéro de port est 1234.</span><span class="sxs-lookup"><span data-stu-id="aed8d-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="aed8d-133">Lorsque vous exécutez l’application, vous verrez un numéro de port différent.</span><span class="sxs-lookup"><span data-stu-id="aed8d-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="aed8d-134">Selon la taille de la fenêtre de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour voir la **maison**, **Environ**, **Contact**, **Inscrivez-vous** et **Connectez-vous dans** les liens.</span><span class="sxs-lookup"><span data-stu-id="aed8d-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="aed8d-135">Mise en place de SSL dans le projet</span><span class="sxs-lookup"><span data-stu-id="aed8d-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="aed8d-136">Pour vous connecter à des fournisseurs d’authentification comme Google et Facebook, vous devrez configurer IIS-Express pour utiliser SSL.</span><span class="sxs-lookup"><span data-stu-id="aed8d-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="aed8d-137">Il est important de continuer à utiliser SSL après la connexion et de ne pas repoinuer sur HTTP, votre cookie de connexion est tout aussi secret que votre nom d’utilisateur et mot de passe, et sans utiliser SSL vous l’envoyez en texte clair sur le fil.</span><span class="sxs-lookup"><span data-stu-id="aed8d-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="aed8d-138">En outre, vous avez déjà pris le temps d’effectuer la poignée de main et sécuriser le canal (qui est la majeure partie de ce qui rend HTTPS plus lent que HTTPS) avant le pipeline MVC est exécuté, afin de rediriger vers HTTP après que vous êtes connecté ne fera pas la demande actuelle ou les demandes futures beaucoup plus rapide.</span><span class="sxs-lookup"><span data-stu-id="aed8d-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="aed8d-139">Dans **Solution Explorer**, cliquez sur le projet **MvcAuth.**</span><span class="sxs-lookup"><span data-stu-id="aed8d-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="aed8d-140">Appuyez sur la clé F4 pour afficher les propriétés du projet.</span><span class="sxs-lookup"><span data-stu-id="aed8d-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="aed8d-141">Alternativement, à partir du menu **View,** vous pouvez sélectionner **Properties Window**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="aed8d-142">Changer **SSL Enabled** to True.</span><span class="sxs-lookup"><span data-stu-id="aed8d-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="aed8d-143">Copiez l’URL SSL `https://localhost:44300/` (qui sera sauf si vous avez créé d’autres projets SSL).</span><span class="sxs-lookup"><span data-stu-id="aed8d-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="aed8d-144">Dans **Solution Explorer**, cliquez à droite sur le projet **MvcAuth** et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="aed8d-145">Sélectionnez l’onglet **Web,** puis collez l’URL SSL dans la boîte **d’Url de projet.**</span><span class="sxs-lookup"><span data-stu-id="aed8d-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="aed8d-146">Enregistrer le fichier (Ctl-S).</span><span class="sxs-lookup"><span data-stu-id="aed8d-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="aed8d-147">Vous aurez besoin de cette URL pour configurer les applications d’authentification Facebook et Google.</span><span class="sxs-lookup"><span data-stu-id="aed8d-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="aed8d-148">Ajoutez l’attribut [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) au `Home` contrôleur pour exiger que toutes les demandes doivent utiliser HTTPS.</span><span class="sxs-lookup"><span data-stu-id="aed8d-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="aed8d-149">Une approche plus sûre consiste à ajouter le filtre [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) à l’application.</span><span class="sxs-lookup"><span data-stu-id="aed8d-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="aed8d-150">Voir la &quot;section Protéger l’application avec SSL et l’attribut d’autorisation&quot; dans mon tutoriel Créer une application ASP.NET [MVC avec auth et SQL DB et déployer à Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="aed8d-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="aed8d-151">Une partie du contrôleur à domicile est indiquée ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="aed8d-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="aed8d-152">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="aed8d-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="aed8d-153">Si vous avez installé le certificat dans le passé, vous pouvez sauter le reste de cette section et sauter à [la création d’une application Google pour OAuth 2 et la connexion de l’application au projet](#goog), sinon, suivez les instructions pour faire confiance au certificat auto-signé qu’IIS Express a généré.</span><span class="sxs-lookup"><span data-stu-id="aed8d-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="aed8d-154">Lisez le dialogue **d’avertissement de sécurité** et cliquez ensuite sur **Oui** si vous voulez installer le certificat représentant localhost.</span><span class="sxs-lookup"><span data-stu-id="aed8d-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="aed8d-155">IE affiche la *page d’accueil* sans avertissement SSL.</span><span class="sxs-lookup"><span data-stu-id="aed8d-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="aed8d-156">Google Chrome accepte également le certificat et affichera le contenu HTTPS sans avertissement.</span><span class="sxs-lookup"><span data-stu-id="aed8d-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="aed8d-157">Firefox utilise son propre magasin de certificats, de sorte qu’il affichera un avertissement.</span><span class="sxs-lookup"><span data-stu-id="aed8d-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="aed8d-158">Pour notre application, vous pouvez cliquer en toute sécurité **sur je comprends les risques**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="aed8d-159">Création d’une application Google pour OAuth 2 et connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="aed8d-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="aed8d-160">Pour les instructions actuelles de Google OAuth, voir [Configuring Google authentification dans ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="aed8d-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="aed8d-161">Accédez à la [Console développeur de Google](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="aed8d-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="aed8d-162">Si vous n’avez pas créé de projet auparavant, sélectionnez **les informations d’identification** dans l’onglet gauche, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="aed8d-163">Dans l’onglet gauche, cliquez sur **Credentials**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="aed8d-164">Cliquez **sur Créer des informations d’identification** puis **OAuth id client**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="aed8d-165">Dans le dialogue **Create Client ID,** conservez l’application **Web** par défaut pour le type d’application.</span><span class="sxs-lookup"><span data-stu-id="aed8d-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="aed8d-166">Définissez les origines **JavaScript autorisées** à l’URL SSL que vous avez utilisée ci-dessus (sauf`https://localhost:44300/` si vous avez créé d’autres projets SSL)</span><span class="sxs-lookup"><span data-stu-id="aed8d-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="aed8d-167">Définissez **l’URI rediriger autorisé** vers :</span><span class="sxs-lookup"><span data-stu-id="aed8d-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="aed8d-168">Cliquez sur l’élément du menu d’écran OAuth Consent, puis réglez votre adresse e-mail et votre nom de produit.</span><span class="sxs-lookup"><span data-stu-id="aed8d-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="aed8d-169">Lorsque vous avez terminé le formulaire cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="aed8d-170">Cliquez sur l’élément menu de la Bibliothèque, **recherchez l’API googleMD,** cliquez dessus, puis appuyez sur Enable.</span><span class="sxs-lookup"><span data-stu-id="aed8d-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="aed8d-171">L’image ci-dessous montre les API activées.</span><span class="sxs-lookup"><span data-stu-id="aed8d-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="aed8d-172">Du gestionnaire de l’API de Google, visitez **l’onglet Credentials** pour obtenir **l’ID client**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="aed8d-173">Téléchargez pour enregistrer un fichier JSON avec des secrets d’application.</span><span class="sxs-lookup"><span data-stu-id="aed8d-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="aed8d-174">Copiez et collez le **ClientId** `UseGoogleAuthentication` et **ClientSecret** dans la méthode trouvée dans le fichier *Startup.Auth.cs* dans le dossier *App_Start.*</span><span class="sxs-lookup"><span data-stu-id="aed8d-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="aed8d-175">Les valeurs **ClientId** et **ClientSecret** indiquées ci-dessous sont des échantillons et ne fonctionnent pas.</span><span class="sxs-lookup"><span data-stu-id="aed8d-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="aed8d-176">Sécurité - Ne stockez jamais de données sensibles dans votre code source.</span><span class="sxs-lookup"><span data-stu-id="aed8d-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="aed8d-177">Le compte et les informations d’identification sont ajoutés au code ci-dessus pour garder l’échantillon simple.</span><span class="sxs-lookup"><span data-stu-id="aed8d-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="aed8d-178">Voir [les meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles à ASP.NET et Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="aed8d-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="aed8d-179">Appuyez sur **CTRL-F5** pour construire et exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="aed8d-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="aed8d-180">Cliquez sur le lien **Ouvrir une session** .</span><span class="sxs-lookup"><span data-stu-id="aed8d-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="aed8d-181">Sous **l’utilisation d’un autre service pour vous connecter,** cliquez sur **Google**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="aed8d-182">Si vous manquez l’une des étapes ci-dessus, vous obtiendrez une erreur HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="aed8d-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="aed8d-183">Revérifier vos pas ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="aed8d-183">Recheck your steps above.</span></span> <span data-ttu-id="aed8d-184">Si vous manquez un paramètre requis (par exemple **le nom du produit),** ajoutez l’article manquant et enregistrez; il peut prendre quelques minutes pour que l’authentification fonctionne.</span><span class="sxs-lookup"><span data-stu-id="aed8d-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="aed8d-185">Vous serez redirigé vers le site Google où vous entrerez vos informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="aed8d-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="aed8d-186">Après avoir entré vos informations d’identification, vous êtes invité à accorder des autorisations pour l’application Web que vous venez de créer : </span><span class="sxs-lookup"><span data-stu-id="aed8d-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="aed8d-187">Cliquez sur **Accepter**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-187">Click **Accept**.</span></span> <span data-ttu-id="aed8d-188">Vous serez désormais redirigé vers la page **Registre** de l’application MvcAuth où vous pouvez enregistrer votre compte Google.</span><span class="sxs-lookup"><span data-stu-id="aed8d-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="aed8d-189">Vous avez la possibilité de changer le nom d'inscription local utilisé pour votre compte Gmail, mais, en règle générale, les utilisateurs préfèrent conserver l'alias de messagerie par défaut (c'est-à-dire celui que vous avez utilisé pour l'authentification).</span><span class="sxs-lookup"><span data-stu-id="aed8d-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="aed8d-190">Cliquez sur **S'inscrire**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="aed8d-191">Création de l’application dans Facebook et connexion de l’application au projet</span><span class="sxs-lookup"><span data-stu-id="aed8d-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="aed8d-192">Pour les instructions actuelles d’authentification Facebook OAuth2, voir [Configuring Facebook authentification](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="aed8d-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="aed8d-193">Examiner les données d’adhésion</span><span class="sxs-lookup"><span data-stu-id="aed8d-193">Examine the Membership Data</span></span>

<span data-ttu-id="aed8d-194">Dans le menu **View,** cliquez sur **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="aed8d-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, cliquez à droite **AspNetUsers** et cliquez sur **Show Table Data**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![données de table aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="aed8d-197">Ajout de données de profil à la classe utilisateur</span><span class="sxs-lookup"><span data-stu-id="aed8d-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="aed8d-198">Dans cette section, vous ajouterez la date de naissance et la ville natale aux données utilisateur lors de l’enregistrement, comme indiqué dans l’image suivante.</span><span class="sxs-lookup"><span data-stu-id="aed8d-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg avec la ville natale et Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="aed8d-200">Ouvrez le fichier *Models-IdentityModels.cs* et ajoutez la date de naissance et les propriétés de la ville natale :</span><span class="sxs-lookup"><span data-stu-id="aed8d-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="aed8d-201">Ouvrez le fichier *Models-AccountViewModels.cs* et la date `ExternalLoginConfirmationViewModel`de naissance définie et les propriétés de la ville d’origine dans .</span><span class="sxs-lookup"><span data-stu-id="aed8d-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="aed8d-202">Ouvrez le fichier *Controllers-AccountController.cs* et ajoutez du `ExternalLoginConfirmation` code pour la date de naissance et la ville d’origine dans la méthode d’action comme indiqué :</span><span class="sxs-lookup"><span data-stu-id="aed8d-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="aed8d-203">Ajoutez la date de naissance et la ville natale au fichier *Views-Account-ExternalLoginConfirmation.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="aed8d-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="aed8d-204">Supprimez la base de données d’adhésion afin que vous puissiez à nouveau enregistrer votre compte Facebook avec votre application et vérifier que vous pouvez ajouter la nouvelle date de naissance et les informations de profil de ville d’origine.</span><span class="sxs-lookup"><span data-stu-id="aed8d-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="aed8d-205">De **Solution Explorer**, cliquez sur **l’icône Afficher tous les fichiers,** puis cliquez à droite *Ajouter\_Data-aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* et cliquez sur **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="aed8d-206">À partir du menu **Tools,** cliquez sur **NuGet Package Manger**, puis cliquez sur **Package Manager Console** (PMC).</span><span class="sxs-lookup"><span data-stu-id="aed8d-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="aed8d-207">Entrez les commandes suivantes dans le PMC.</span><span class="sxs-lookup"><span data-stu-id="aed8d-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="aed8d-208">Activer les migrations</span><span class="sxs-lookup"><span data-stu-id="aed8d-208">Enable-Migrations</span></span>
2. <span data-ttu-id="aed8d-209">Add-Migration Init</span><span class="sxs-lookup"><span data-stu-id="aed8d-209">Add-Migration Init</span></span>
3. <span data-ttu-id="aed8d-210">Mise à jour-Base de données</span><span class="sxs-lookup"><span data-stu-id="aed8d-210">Update-Database</span></span>

<span data-ttu-id="aed8d-211">Exécutez l’application et utilisez FaceBook et Google pour vous connecter et enregistrer certains utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="aed8d-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="aed8d-212">Examiner les données d’adhésion</span><span class="sxs-lookup"><span data-stu-id="aed8d-212">Examine the Membership Data</span></span>

<span data-ttu-id="aed8d-213">Dans le menu **View,** cliquez sur **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="aed8d-214">Cliquez à droite **AspNetUsers** et cliquez sur **Afficher les données de la table**.</span><span class="sxs-lookup"><span data-stu-id="aed8d-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="aed8d-215">Les `HomeTown` `BirthDate` champs et les champs sont indiqués ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="aed8d-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="aed8d-216">Vous vous connectez à votre application et vous connectez-vous avec un autre compte</span><span class="sxs-lookup"><span data-stu-id="aed8d-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="aed8d-217">Si vous vous connectez à votre application avec Facebook, puis vous vous déconnectez et essayez de vous connecter à nouveau avec un autre compte Facebook (à l’aide du même navigateur), vous serez immédiatement connecté au compte Facebook précédent que vous avez utilisé.</span><span class="sxs-lookup"><span data-stu-id="aed8d-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="aed8d-218">Afin d’utiliser un autre compte, vous devez naviguer vers Facebook et vous connecter à Facebook.</span><span class="sxs-lookup"><span data-stu-id="aed8d-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="aed8d-219">La même règle s’applique à tout autre fournisseur d’authentification de la 3e partie.</span><span class="sxs-lookup"><span data-stu-id="aed8d-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="aed8d-220">Alternativement, vous pouvez vous connecter avec un autre compte en utilisant un navigateur différent.</span><span class="sxs-lookup"><span data-stu-id="aed8d-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aed8d-221">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="aed8d-221">Next Steps</span></span>

<span data-ttu-id="aed8d-222">Voir [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span><span class="sxs-lookup"><span data-stu-id="aed8d-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="aed8d-223">Consultez les boutons de connexion sociale de Jerrie pour ASP.NET MVC 5 pour obtenir des boutons de connexion sociale.</span><span class="sxs-lookup"><span data-stu-id="aed8d-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="aed8d-224">Suivez mon tutoriel [Créer une application MVC ASP.NET avec auth et SQL DB et déployer à Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), qui continue ce tutoriel et montre ce qui suit:</span><span class="sxs-lookup"><span data-stu-id="aed8d-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="aed8d-225">Comment déployer votre application sur Azure.</span><span class="sxs-lookup"><span data-stu-id="aed8d-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="aed8d-226">Comment sécuriser votre application avec des rôles.</span><span class="sxs-lookup"><span data-stu-id="aed8d-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="aed8d-227">Comment sécuriser votre application avec les [Filtres RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) et [autoriser.](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="aed8d-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="aed8d-228">Comment utiliser l’API d’adhésion pour ajouter des utilisateurs et des rôles.</span><span class="sxs-lookup"><span data-stu-id="aed8d-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="aed8d-229">S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer.</span><span class="sxs-lookup"><span data-stu-id="aed8d-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="aed8d-230">Vous pouvez également demander de nouveaux sujets à [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="aed8d-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="aed8d-231">Vous pouvez même demander et voter sur de nouvelles fonctionnalités à ajouter à ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="aed8d-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="aed8d-232">Par exemple, vous pouvez voter pour un outil pour créer et gérer les [utilisateurs et les rôles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="aed8d-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="aed8d-233">Pour une bonne explication du fonctionnement de ASP.NET Services d’authentification externe, consultez [les services d’authentification externe](https://asp.net/web-api/overview/security/external-authentication-services)de Robert McMurray .</span><span class="sxs-lookup"><span data-stu-id="aed8d-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="aed8d-234">L’article de Robert entre également dans les détails en permettant l’authentification microsoft et Twitter.</span><span class="sxs-lookup"><span data-stu-id="aed8d-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="aed8d-235">L’excellent [tutoriel EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) de Tom Dykstra montre comment travailler avec le Cadre d’entités.</span><span class="sxs-lookup"><span data-stu-id="aed8d-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
