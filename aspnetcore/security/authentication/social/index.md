---
title: 'Authentification à l’aide de fournisseurs externes (Facebook, Google et autres) dans ASP.NET Core'
author: rick-anderson
description: Ce didacticiel montre comment générer une application ASP.NET Core 2.x à l’aide d’OAuth 2.0 avec des fournisseurs d’authentification externes.
ms.author: riande
ms.custom: mvc
ms.date: 1/19/2019
uid: security/authentication/social/index
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="42548-103">Authentification à l’aide de fournisseurs externes (Facebook, Google et autres) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42548-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="42548-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42548-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="42548-105">Ce tutoriel montre comment créer une application ASP.NET Core 2.2 qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2.0 avec des informations d’identification provenant de fournisseurs d’authentification externes.</span><span class="sxs-lookup"><span data-stu-id="42548-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="42548-106">Les fournisseurs [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) et [Microsoft](xref:security/authentication/microsoft-logins) sont traités dans les sections qui suivent.</span><span class="sxs-lookup"><span data-stu-id="42548-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="42548-107">D’autres fournisseurs sont disponibles dans des packages tiers, comme [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) et [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="42548-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Icônes de réseau social pour Facebook, Twitter, Google plus et Windows](index/_static/social.png)

<span data-ttu-id="42548-109">Permettre aux utilisateurs de se connecter avec leurs informations d’identification existantes est pratique pour eux et transfère une grande partie de la complexité de la gestion du processus de connexion sur un tiers.</span><span class="sxs-lookup"><span data-stu-id="42548-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="42548-110">Pour obtenir des exemples de la façon dont les connexions des réseaux sociaux peuvent amener du trafic et des conversions de clients, consultez les études de cas par [Facebook](https://www.facebook.com/unsupportedbrowser) et [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="42548-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="42548-111">Créer un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42548-111">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="42548-112">Dans Visual Studio 2017, créez un projet à partir de la page de démarrage, ou via **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="42548-112">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="42548-113">Sélectionnez le modèle **Application web ASP.NET Core** disponible dans la catégorie **Visual C#** > **.NET Core** :</span><span class="sxs-lookup"><span data-stu-id="42548-113">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>
* <span data-ttu-id="42548-114">Sélectionnez **Modifier l’authentification** et définissez l’authentification sur **Comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="42548-114">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="42548-115">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="42548-115">Apply migrations</span></span>

* <span data-ttu-id="42548-116">Exécutez l’application et sélectionnez le lien **S’inscrire**.</span><span class="sxs-lookup"><span data-stu-id="42548-116">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="42548-117">Entrez l’adresse e-mail et le mot de passe du nouveau compte, puis sélectionnez **S’inscrire**.</span><span class="sxs-lookup"><span data-stu-id="42548-117">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="42548-118">Suivez les instructions pour appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="42548-118">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="42548-119">Utilisez SecretManager pour stocker les jetons affectés par les fournisseurs de connexion</span><span class="sxs-lookup"><span data-stu-id="42548-119">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="42548-120">Les fournisseurs de connexion de réseaux sociaux affectent des jetons **ID d’application** et **Secret de l’application** lors du processus d’inscription.</span><span class="sxs-lookup"><span data-stu-id="42548-120">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="42548-121">Les noms de jeton exacts varient selon le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="42548-121">The exact token names vary by provider.</span></span> <span data-ttu-id="42548-122">Ces jetons représentent les informations d’identification que votre application utilise pour accéder à son API.</span><span class="sxs-lookup"><span data-stu-id="42548-122">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="42548-123">Les jetons constituent les « secrets » qui peuvent être liés à la configuration de votre application à l’aide de [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="42548-123">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="42548-124">Secret Manager est une alternative plus sécurisée pour stocker les jetons dans un fichier de configuration, tel que *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="42548-124">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42548-125">Secret Manager est uniquement réservé au développement.</span><span class="sxs-lookup"><span data-stu-id="42548-125">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="42548-126">Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="42548-126">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="42548-127">Suivez les étapes de la rubrique [Stockage sécurisé des secrets d’application lors du développement dans ASP.NET Core](xref:security/app-secrets) pour stocker les jetons affectés par chaque fournisseur de connexion ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="42548-127">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="42548-128">Configurer les fournisseurs de connexion nécessaires à votre application</span><span class="sxs-lookup"><span data-stu-id="42548-128">Setup login providers required by your application</span></span>

<span data-ttu-id="42548-129">Utilisez les rubriques suivantes pour configurer votre application pour utiliser ces différents fournisseurs :</span><span class="sxs-lookup"><span data-stu-id="42548-129">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="42548-130">Instructions pour [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="42548-130">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="42548-131">Instructions pour [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="42548-131">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="42548-132">Instructions pour [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="42548-132">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="42548-133">Instructions pour [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="42548-133">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="42548-134">Instructions pour les [autres fournisseurs](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="42548-134">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="42548-135">Définition facultative d’un mot de passe</span><span class="sxs-lookup"><span data-stu-id="42548-135">Optionally set password</span></span>

<span data-ttu-id="42548-136">Quand vous vous inscrivez auprès d’un fournisseur de connexion externe, vous n’avez pas de mot de passe inscrit auprès de l’application.</span><span class="sxs-lookup"><span data-stu-id="42548-136">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="42548-137">Ceci vous évite de devoir créer et mémoriser un mot de passe pour le site, mais vous rend aussi dépendant du fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="42548-137">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="42548-138">Si le fournisseur de connexion externe est indisponible, vous ne pouvez pas vous connecter au site web.</span><span class="sxs-lookup"><span data-stu-id="42548-138">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="42548-139">Pour créer un mot de passe et vous connecter à l’aide de l’e-mail que vous avez défini lors du processus de connexion avec des fournisseurs externes :</span><span class="sxs-lookup"><span data-stu-id="42548-139">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="42548-140">Sélectionnez le lien **Bonjour &lt;alias d’e-mail&gt;** en haut à droite pour accéder à la vue **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="42548-140">Select the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Vue Gérer de l’application web](index/_static/pass1a.png)

* <span data-ttu-id="42548-142">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="42548-142">Select **Create**</span></span>

![Page Définir votre mot de passe](index/_static/pass2a.png)

* <span data-ttu-id="42548-144">Définissez un mot de passe valide à utiliser pour vous connecter avec votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="42548-144">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42548-145">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="42548-145">Next steps</span></span>

* <span data-ttu-id="42548-146">Cet article a présenté l’authentification externe et expliqué les prérequis nécessaires pour ajouter des connexions externes à votre application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="42548-146">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="42548-147">Référencez les pages spécifiques au fournisseur pour configurer les connexions pour les fournisseurs nécessaires à votre application.</span><span class="sxs-lookup"><span data-stu-id="42548-147">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="42548-148">Vous souhaiterez peut-être conserver des données supplémentaires relatives à l’utilisateur et à ses jetons d’accès et d’actualisation.</span><span class="sxs-lookup"><span data-stu-id="42548-148">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="42548-149">Pour plus d'informations, consultez <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="42548-149">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
