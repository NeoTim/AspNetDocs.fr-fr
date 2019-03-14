---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: ASP.NET (VB) d’Options d’hébergement | Microsoft Docs
author: rick-anderson
description: Les applications web ASP.NET sont généralement conçues, créé, testé dans un environnement de développement local et doivent être déployées pour un o d’environnement de production...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 95df9c7595dc10f2bfce44845236d11457b8b3cb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031136"
---
<a name="aspnet-hosting-options-vb"></a><span data-ttu-id="d3f87-103">Options d’hébergement ASP.NET (VB)</span><span class="sxs-lookup"><span data-stu-id="d3f87-103">ASP.NET Hosting Options (VB)</span></span>
====================
<span data-ttu-id="d3f87-104">par [Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="d3f87-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

[<span data-ttu-id="d3f87-105">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="d3f87-105">Download PDF</span></span>](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> <span data-ttu-id="d3f87-106">Les applications web ASP.NET sont généralement conçues, créées et testées dans un environnement de développement local et un doivent être déployées dans un environnement de production dès qu’elle sera prête à être publiée.</span><span class="sxs-lookup"><span data-stu-id="d3f87-106">ASP.NET web applications are typically designed, created, and tested in a local development environment and need to be deployed to a production environment once it is ready for release.</span></span> <span data-ttu-id="d3f87-107">Ce didacticiel fournit une vue d’ensemble du processus de déploiement et sert d’introduction à cette série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="d3f87-107">This tutorial provides a high-level overview of the deployment process and serves as an introduction to this tutorial series.</span></span>


## <a name="introduction"></a><span data-ttu-id="d3f87-108">Introduction</span><span class="sxs-lookup"><span data-stu-id="d3f87-108">Introduction</span></span>

<span data-ttu-id="d3f87-109">Applications Web sont généralement conçues, créées et testées dans un environnement de développement qui est accessible uniquement pour les programmeurs travaillant sur le site.</span><span class="sxs-lookup"><span data-stu-id="d3f87-109">Web applications are typically designed, created, and tested in a development environment that is accessible only to the programmers working on the site.</span></span> <span data-ttu-id="d3f87-110">Une fois que l’application est prête à être publiée, elle est déplacée vers un environnement de production où le site est accessible par toute personne sur Internet.</span><span class="sxs-lookup"><span data-stu-id="d3f87-110">Once the application is ready to be released, it is moved to a production environment where the site can be accessed by anyone on the Internet.</span></span> <span data-ttu-id="d3f87-111">Ce processus de déploiement introduit un certain nombre de défis :</span><span class="sxs-lookup"><span data-stu-id="d3f87-111">This deployment process introduces a number of challenges:</span></span>

- <span data-ttu-id="d3f87-112">Un environnement de production doit exister et être correctement le programme d’installation avant de pouvoir déployer une application ASP.NET ; en outre, l’environnement de production doit rester à jour avec les derniers correctifs de sécurité.</span><span class="sxs-lookup"><span data-stu-id="d3f87-112">A production environment must exist and be properly setup before an ASP.NET application can be deployed; moreover, the production environment must be kept up to date with the latest security patches.</span></span>
- <span data-ttu-id="d3f87-113">Le jeu approprié des fichiers de balisage, les fichiers de code et les fichiers de prise en charge doit être copié à partir de l’environnement de développement à l’environnement de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-113">The correct set of markup files, code files, and support files must be copied from the development environment to the production environment.</span></span> <span data-ttu-id="d3f87-114">Pour les applications pilotées par les données, vous devrez peut-être copier le schéma de base de données et/ou les données, ainsi.</span><span class="sxs-lookup"><span data-stu-id="d3f87-114">For data-driven applications, this might require copying the database schema and/or data, as well.</span></span>
- <span data-ttu-id="d3f87-115">Il peut exister des différences de configuration entre les deux environnements.</span><span class="sxs-lookup"><span data-stu-id="d3f87-115">There may be configuration differences between the two environments.</span></span> <span data-ttu-id="d3f87-116">Le serveur de chaîne ou un e-mail de connexion de base de données utilisé dans l’environnement de développement sera probablement différent de celle de l’environnement de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-116">The database connection string or email server used in the development environment will likely be different than the production environment.</span></span> <span data-ttu-id="d3f87-117">De plus, le comportement de l’application peut dépendre de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="d3f87-117">What's more, the behavior of the application may depend on the environment.</span></span> <span data-ttu-id="d3f87-118">Par exemple, lorsqu’une erreur se produit dans le développement détails de l’erreur peuvent être affichés à l’écran, mais lorsqu’une erreur se produit en production, une page d’erreur convivial doit être affichée à la place, et les détails d’erreur envoyé par courrier électronique pour les développeurs.</span><span class="sxs-lookup"><span data-stu-id="d3f87-118">For example, when an error occurs in development the error's details can be displayed on screen, but when an error occurs in production, a user-friendly error page should be displayed instead, and the error details emailed to the developers.</span></span>

<span data-ttu-id="d3f87-119">Pour la transmission de la première difficulté - configuration et maintenance d’un environnement de production - nombreuses personnes et entreprises externaliser leurs environnements de production à *web des fournisseurs d’hébergement*.</span><span class="sxs-lookup"><span data-stu-id="d3f87-119">To obviate the first challenge - setting up and maintaining a production environment - many individuals and businesses outsource their production environments to *web hosting providers*.</span></span> <span data-ttu-id="d3f87-120">Un fournisseur d’hébergement web est une société qui gère l’environnement de production à votre place.</span><span class="sxs-lookup"><span data-stu-id="d3f87-120">A web hosting provider is a company that manages the production environment on your behalf.</span></span> <span data-ttu-id="d3f87-121">Il existe un nombre incalculable de web des fournisseurs d’hébergement, chacune avec différents prix et les niveaux de service ; consultez la section « Recherche d’un fournisseur d’hébergement Web » pour obtenir des conseils sur la localisation d’un fournisseur de services de ce type.</span><span class="sxs-lookup"><span data-stu-id="d3f87-121">There are countless web host providers, each with varying prices and service levels; see the "Finding a Web Host Provider" section for tips on locating such a service provider.</span></span>

<span data-ttu-id="d3f87-122">Il s’agit de la première d’une série de didacticiels qui examinent les étapes impliquées dans le déploiement d’une application web ASP.NET dans un environnement de production gérée par un fournisseur d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="d3f87-122">This is the first in a series of tutorials that look at the steps involved in deploying an ASP.NET web application to a production environment managed by a web host provider.</span></span> <span data-ttu-id="d3f87-123">Au cours de ces didacticiels, nous allons examiner :</span><span class="sxs-lookup"><span data-stu-id="d3f87-123">Over the course of these tutorials we will examine:</span></span>

- <span data-ttu-id="d3f87-124">Les fichiers devant être déployés sur le fournisseur d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="d3f87-124">What files need to be deployed to the web host provider.</span></span>
- <span data-ttu-id="d3f87-125">Outils pour rationaliser le processus de déploiement.</span><span class="sxs-lookup"><span data-stu-id="d3f87-125">Tools for streamlining the deployment process.</span></span>
- <span data-ttu-id="d3f87-126">Comment déployer une base de données.</span><span class="sxs-lookup"><span data-stu-id="d3f87-126">How to deploy a database.</span></span>
- <span data-ttu-id="d3f87-127">Conseils pour le déploiement d’une base de données qui utilise [le fournisseur d’appartenances SQL et les rôles](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), ainsi que la manière d’imiter l’outil d’Administration de site Web dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-127">Tips for deploying a database that uses [the SQL-based Membership and Roles provider](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), along with ways to mimic the Website Administration Tool in a production environment.</span></span>
- <span data-ttu-id="d3f87-128">Stratégies de mise à jour sans heurts de la base de données en production avec les modifications apportées au cours du développement.</span><span class="sxs-lookup"><span data-stu-id="d3f87-128">Strategies for smoothly updating the database in production with changes made during development.</span></span>
- <span data-ttu-id="d3f87-129">Techniques pour la journalisation des erreurs qui se produisent sur la production et des méthodes pour avertir les développeurs lorsqu’une erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="d3f87-129">Techniques for logging errors that occur on production, and ways to notify developers when an error occurs.</span></span>

<span data-ttu-id="d3f87-130">Ces didacticiels sont conçues pour être concis et fournir des instructions pas à pas avec de nombreuses captures d’écran pour vous guident tout au long du processus visuellement.</span><span class="sxs-lookup"><span data-stu-id="d3f87-130">These tutorials are geared to be concise and to provide step-by-step instructions with plenty of screen shots to walk you through the process visually.</span></span> <span data-ttu-id="d3f87-131">Ce didacticiel inaugurale fournit une vue d’ensemble du processus de déploiement ASP.NET et des conseils sur la recherche d’un fournisseur d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="d3f87-131">This inaugural tutorial provides an overview of the ASP.NET deployment process and advice on finding a web hosting provider.</span></span> <span data-ttu-id="d3f87-132">C’est parti !</span><span class="sxs-lookup"><span data-stu-id="d3f87-132">Let's get started!</span></span>

## <a name="an-overview-of-the-aspnet-deployment-process"></a><span data-ttu-id="d3f87-133">Une vue d’ensemble du processus de déploiement ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d3f87-133">An Overview of the ASP.NET Deployment Process</span></span>

<span data-ttu-id="d3f87-134">En bref, le déploiement d’une application ASP.NET implique les trois étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d3f87-134">In a nutshell, deploying an ASP.NET application involves the following three steps:</span></span>

1. <span data-ttu-id="d3f87-135">Configurer l’application web, le serveur web et la base de données dans l’environnement de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-135">Configure the web application, web server, and database in the production environment.</span></span>
2. <span data-ttu-id="d3f87-136">Synchroniser les pages ASP.NET, les fichiers de code, les assemblys dans le `Bin` dossier et les fichiers de prise en charge de HTML associés tels que les fichiers CSS et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3f87-136">Synchronize the ASP.NET pages, code files, the assemblies in the `Bin` folder, and HTML-related support files like CSS and JavaScript files.</span></span>
3. <span data-ttu-id="d3f87-137">Synchroniser le schéma de base de données et/ou les données.</span><span class="sxs-lookup"><span data-stu-id="d3f87-137">Synchronize the database schema and/or data.</span></span>

<span data-ttu-id="d3f87-138">Les informations de configuration pour une application web se trouve généralement dans le `Web.config` de fichiers et inclut des chaînes de connexion de base de données, critères de la gestion des erreurs, URL de réécriture des règles et les informations de serveur de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d3f87-138">The configuration information for a web application is typically located in the `Web.config` file, and includes database connection strings, error handling criteria, URL rewriting rules, and email server information.</span></span> <span data-ttu-id="d3f87-139">Souvent, ces informations sont différentes pour une application dans le développement par rapport à la même application en production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-139">Oftentimes this information is different for an application in development versus the same application in production.</span></span> <span data-ttu-id="d3f87-140">Par exemple, lorsque vous développez une application, il est préférable d’utiliser une base de données de développement afin que vous ne testez pas sur la base de données de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-140">For instance, when developing an application it's best to use a development database so that you're not testing against the production database.</span></span> <span data-ttu-id="d3f87-141">Par conséquent, les chaînes de connexion de base de données diffèrent généralement entre les applications de développement et de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-141">As a result, the database connection strings typically differ between development and production applications.</span></span> <span data-ttu-id="d3f87-142">En raison de ces différences, partie de déploiement implique d’apporter des modifications aux informations de configuration de l’application web.</span><span class="sxs-lookup"><span data-stu-id="d3f87-142">Due to these differences, part of deployment involves making changes to the web application's configuration information.</span></span>

<span data-ttu-id="d3f87-143">En plus des modifications de configuration d’application web, étape 1 peut également impliquer le configuration pour le serveur web et de la base de données.</span><span class="sxs-lookup"><span data-stu-id="d3f87-143">In addition to web application configuration changes, Step 1 also may entail configuration for the web server and database.</span></span> <span data-ttu-id="d3f87-144">Par exemple, si une page ASP.NET crée ou supprime des fichiers d’un répertoire sur le serveur web du serveur web doit être configuré pour autoriser ces modifications du système de fichiers.</span><span class="sxs-lookup"><span data-stu-id="d3f87-144">For example, if an ASP.NET page creates or deletes files from a directory on the web server then the web server needs to be configured to permit these file system modifications.</span></span> <span data-ttu-id="d3f87-145">De même, il peut exister des paramètres d’autorisation ou d’authentification qui doivent être apportées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="d3f87-145">Similarly, there may be permission or authentication settings that need to be made to the database.</span></span>


<span data-ttu-id="d3f87-146">Étape 2 implique la synchronisation de l’ensemble d’essential ASP.NET pages et les fichiers de prise en charge entre les environnements de développement et de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-146">Step 2 involves synchronizing the set of essential ASP.NET pages and support files between the development and production environments.</span></span> <span data-ttu-id="d3f87-147">L’ensemble de ASP. NET-fichiers qui doivent être synchronisées entre les deux environnements varie selon le type de projet créé dans Visual Studio et est la discussion dans le didacticiel suivant,  <em>[déterminer quels fichiers doivent être déployés](determining-what-files-need-to-be-deployed-vb.md)</em>.</span><span class="sxs-lookup"><span data-stu-id="d3f87-147">The particular set of ASP.NET-related files that need to be synchronized between the two environments depends on the type of project you created in Visual Studio, and is the discussion in the next tutorial, <em>[Determining What Files Need to Be Deployed](determining-what-files-need-to-be-deployed-vb.md)</em>.</span></span> <span data-ttu-id="d3f87-148">Les didacticiels troisième et quatrième -  <em>[déploiement de votre Site à l’aide de FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>et <em>[déploiement de votre Site à l’aide de Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> -examiner différents outils et techniques pour la synchronisation de ces fichiers.</span><span class="sxs-lookup"><span data-stu-id="d3f87-148">The third and fourth tutorials - <em>[Deploying Your Site Using FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>and <em>[Deploying Your Site Using Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> - examine different tools and techniques for syncing these files.</span></span>

<span data-ttu-id="d3f87-149">Lors de la création d’applications orientées données, il existe généralement deux bases de données utilisés : un pour le développement et l’autre sur la production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-149">When building data-driven applications there are typically two databases being used: one for development and one on production.</span></span> <span data-ttu-id="d3f87-150">Pendant le développement, schéma de la base de données développement peut être modifié pour inclure les nouvelles tables, colonnes, procédures stockées et déclencheurs, ou peut être modifié pour supprimer ou renommer des objets de base de données existante.</span><span class="sxs-lookup"><span data-stu-id="d3f87-150">During development, the development database's schema may be modified to include new tables, columns, stored procedures, and triggers, or may be modified to remove or rename existing database objects.</span></span> <span data-ttu-id="d3f87-151">Entre le moment où ces modifications sont apportées et l’heure de que l’application est déployée en production, les bases de données de développement et de production ne sont pas synchronisés. Ce comportement asynchrone doit être fixe pendant le processus de déploiement.</span><span class="sxs-lookup"><span data-stu-id="d3f87-151">Between the time that these changes are made and the time the application is deployed to production, the development and production databases are out of sync. This asynchrony needs to be fixed during the deployment process.</span></span> <span data-ttu-id="d3f87-152">Ces défis seront examinées dans les didacticiels futures.</span><span class="sxs-lookup"><span data-stu-id="d3f87-152">These challenges will be examined in future tutorials.</span></span>

## <a name="finding-a-web-host-provider"></a><span data-ttu-id="d3f87-153">Recherche d’un fournisseur d’hébergement Web</span><span class="sxs-lookup"><span data-stu-id="d3f87-153">Finding a Web Host Provider</span></span>

<span data-ttu-id="d3f87-154">Les applications ASP.NET peuvent être déployées vers un serveur web qui a le .NET Framework et Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="d3f87-154">ASP.NET applications can be deployed to any web server that has the .NET Framework and Internet Information Services (IIS) installed.</span></span> <span data-ttu-id="d3f87-155">Vous pouvez héberger un site à partir de votre ordinateur personnel, en supposant que vous aviez une connexion haut débit à Internet et au savoir comment configurer votre routeur pour autoriser les demandes web entrantes.</span><span class="sxs-lookup"><span data-stu-id="d3f87-155">You could host a site from your personal computer, assuming you had a broadband connection to the Internet and the know how to configure your router to allow incoming web requests.</span></span> <span data-ttu-id="d3f87-156">Vous pouvez également héberger un site à partir d’un ordinateur dans un intranet, contrairement à de nombreuses entreprises.</span><span class="sxs-lookup"><span data-stu-id="d3f87-156">You could also host a site from a computer in an intranet, as many companies do.</span></span> <span data-ttu-id="d3f87-157">L’objectif de ces didacticiels, toutefois, l’hébergement de votre site Web avec un fournisseur d’hébergement web.</span><span class="sxs-lookup"><span data-stu-id="d3f87-157">The focus of these tutorials, however, is hosting your website with a web host provider.</span></span>

> [!NOTE]
> <span data-ttu-id="d3f87-158">[IIS](https://www.iis.net/) est le serveur web de qualité professionnelle de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d3f87-158">[IIS](https://www.iis.net/) is Microsoft's enterprise-grade web server.</span></span> <span data-ttu-id="d3f87-159">Il est livré avec les éditions de non-accueil de Windows, tels que Windows Server 2008 et certaines éditions de Windows Vista.</span><span class="sxs-lookup"><span data-stu-id="d3f87-159">It ships with the non-Home editions of Windows, such as Windows Server 2008 and certain editions of Windows Vista.</span></span> <span data-ttu-id="d3f87-160">Vous n’avez pas besoin d’installer IIS pour servir des applications ASP.NET dans un environnement de développement, car Visual Studio inclut le serveur Web de développement ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d3f87-160">You do not need to install IIS to serve ASP.NET applications in a development environment, as Visual Studio includes the ASP.NET Development Web Server.</span></span> <span data-ttu-id="d3f87-161">Toutefois, le serveur Web de développement ASP.NET accepte uniquement les connexions locales et ne peut donc pas être utilisé dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-161">However, the ASP.NET Development Web Server only accepts local connections and therefore cannot be used in a production environment.</span></span>


<span data-ttu-id="d3f87-162">Avant de pouvoir déployer votre site à un fournisseur d’hébergement web vous devez d’abord déterminer quelle société commerciale.</span><span class="sxs-lookup"><span data-stu-id="d3f87-162">Before you can deploy your site to a web host provider you must first decide what company to do business with.</span></span> <span data-ttu-id="d3f87-163">Il existe un nombre incalculable web hébergeant des entreprises dans la place de marché ; une recherche « société d’hébergement web » renvoie les résultats de plus de cinq millions.</span><span class="sxs-lookup"><span data-stu-id="d3f87-163">There are countless web hosting companies in the marketplace; a search for "web hosting company" returns more than five million results.</span></span> <span data-ttu-id="d3f87-164">Comment trouver celle qui vous convient ?</span><span class="sxs-lookup"><span data-stu-id="d3f87-164">How do you find the one that's right for you?</span></span> <span data-ttu-id="d3f87-165">Votre moteur de recherche est un bon point de départ, comme les sites Web tels que [TopHosts](http://www.tophosts.com/) et [HostCritique](http://www.hostcritique.net/), qui de comparer différents services d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="d3f87-165">Your favorite search engine is a good starting place, as are websites like [TopHosts](http://www.tophosts.com/) and [HostCritique](http://www.hostcritique.net/), which compare and contrast various hosting services.</span></span> <span data-ttu-id="d3f87-166">Je vous conseille également de poser vos collègues et vos collègues pour les recommandations ; Vous pouvez également demander de recommandations à le [hébergement Open Forum](https://forums.asp.net/158.aspx) ici à la [Forums ASP.NET](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="d3f87-166">I also advise asking your colleagues and coworkers for any recommendations; you can also ask for recommendations at the [Hosting Open Forum](https://forums.asp.net/158.aspx) here at the [ASP.NET Forums](https://forums.asp.net/).</span></span>

<span data-ttu-id="d3f87-167">En général, sociétés d’hébergement Web offrent des plans d’hébergement partagés et dédié de plans d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="d3f87-167">Web hosting companies typically offer shared hosting plans and dedicated hosting plans.</span></span> <span data-ttu-id="d3f87-168">Avec partagé qui héberge un hôte de serveur web unique des dizaines si ce n’est pas des centaines de différents sites Web.</span><span class="sxs-lookup"><span data-stu-id="d3f87-168">With shared hosting a single web server hosts dozens if not hundreds of different websites.</span></span> <span data-ttu-id="d3f87-169">Avec un hébergement dédié vous louez un ordinateur à partir de l’entreprise qui sert votre site et votre site autonome.</span><span class="sxs-lookup"><span data-stu-id="d3f87-169">With dedicated hosting you lease a computer from the company that serves your site and your site alone.</span></span> <span data-ttu-id="d3f87-170">Un plan d’hébergement partagé peut inclure la prise en charge pour les pages ASP.NET, la possibilité de travailler avec les bases de données Microsoft Access, 5 Go d’espace disque et le trafic de la bande passante mensuel de 100 Go pour 9,95 USD par mois.</span><span class="sxs-lookup"><span data-stu-id="d3f87-170">A shared hosting plan might include support for ASP.NET pages, the ability to work with Microsoft Access databases, 5 GB of disk space, and 100 GB of monthly bandwidth traffic for $9.95 per month.</span></span> <span data-ttu-id="d3f87-171">Un autre plan d’hébergement partagé peut inclure la prise en charge pour les pages ASP.NET, l’accès au serveur de base de données Microsoft SQL Server 2008, 10 Go d’espace disque et 250 Go du trafic de la bande passante mensuel pour 19,95 $ par mois.</span><span class="sxs-lookup"><span data-stu-id="d3f87-171">Another shared hosting plan might include support for ASP.NET pages, access to the Microsoft SQL Server 2008 database server, 10 GB of disk space and 250 GB of monthly bandwidth traffic for $19.95 per month.</span></span> <span data-ttu-id="d3f87-172">Dédié de plans d’hébergement sont généralement beaucoup plus onéreuse, d’évaluation des coûts plusieurs dollars par mois, mais offre de meilleures performances et davantage de contrôle que partagé options d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="d3f87-172">Dedicated hosting plans are usually much more expensive, costing several hundred dollars per month, but offer better performance and more control than shared hosting options.</span></span> <span data-ttu-id="d3f87-173">Quel plan que vous choisissez dépend de votre budget, la quantité de trafic votre site Web reçoit et les fonctionnalités que vous pensez que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="d3f87-173">What plan you choose depends on your budget, how much traffic your website receives, and the features you anticipate you'll need.</span></span>

<span data-ttu-id="d3f87-174">Deux considérations importantes lors du choix d’un fournisseur d’hébergement web sont le service clientèle et qualité de service.</span><span class="sxs-lookup"><span data-stu-id="d3f87-174">Two important considerations when choosing a web host provider are customer service and quality of service.</span></span> <span data-ttu-id="d3f87-175">Si vous avez une question ou un problème de configuration, combien de temps faut-il de soumettre votre problème au support technique de l’hôte web jusqu'à ce que vous obtenez une réponse ?</span><span class="sxs-lookup"><span data-stu-id="d3f87-175">If you have a question or a configuration problem, how long does it take from submitting your problem to the web host's helpdesk until you get a response?</span></span> <span data-ttu-id="d3f87-176">La fiabilité sont des services de la société ?</span><span class="sxs-lookup"><span data-stu-id="d3f87-176">How reliable are the company's services?</span></span> <span data-ttu-id="d3f87-177">Ils ont souvent des pannes de base de données ?</span><span class="sxs-lookup"><span data-stu-id="d3f87-177">Do they frequently have database outages?</span></span> <span data-ttu-id="d3f87-178">La fréquence à laquelle leur serveur de messagerie mis hors connexion ?</span><span class="sxs-lookup"><span data-stu-id="d3f87-178">How often does their email server go offline?</span></span> <span data-ttu-id="d3f87-179">Vous pouvez toujours demander une société et fournissent des informations sur les temps de fonctionnement et de vous renseigner sur leur stratégie de service client, mais un moyen plus sûr est de solliciter les commentaires des clients antérieurs et actuels, vous pouvez le faire via les forums en ligne, les groupes de discussion et les serveurs de liste .</span><span class="sxs-lookup"><span data-stu-id="d3f87-179">You can always ask a company to provide details about their uptime and inquire about their customer service policy, but a more surefire way is to solicit the feedback of current and past customers, which you can do through online forums, newsgroups, and email listservs.</span></span>

> [!NOTE]
> <span data-ttu-id="d3f87-180">Certaines entreprises d’hébergement web concentrent leurs activités sur une pile de technologie particulière, telles que .NET ou [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux :, **A** pache, **M** ysql :, et **P** HP), par conséquent, assurez-vous que la société que vous sélectionnez héberge les applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d3f87-180">Some web hosting companies focus their business on a particular technology stack, such as .NET or [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, and **P** HP), so make sure that the company you select hosts ASP.NET applications.</span></span> <span data-ttu-id="d3f87-181">Vérifiez également qu’ils prennent en charge la version d’ASP.NET que vous utilisez pour générer votre application.</span><span class="sxs-lookup"><span data-stu-id="d3f87-181">Also check to ensure that they support the version of ASP.NET you are using to build your application.</span></span> <span data-ttu-id="d3f87-182">Et si vous générez une application orientée données, assurez-vous que l’hôte web offre le même serveur de base de données et la même version que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="d3f87-182">And if you are building a data-driven application, make sure that the web host offers the same database server and version that you are using.</span></span>


## <a name="summary"></a><span data-ttu-id="d3f87-183">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="d3f87-183">Summary</span></span>

<span data-ttu-id="d3f87-184">Les applications web ASP.NET sont généralement conçues, créées et testées dans un environnement de développement local.</span><span class="sxs-lookup"><span data-stu-id="d3f87-184">ASP.NET web applications are typically designed, created, and tested in a local development environment.</span></span> <span data-ttu-id="d3f87-185">Une fois qu’une version est prête à être publiée, elle est déplacée vers un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="d3f87-185">Once a version is ready for release, it is moved to a production environment.</span></span> <span data-ttu-id="d3f87-186">Bien qu’il soit possible d’héberger ASP.NET des sites Web sur votre ordinateur personnel ou des serveurs au sein de votre entreprise, de nombreuses entreprises et individus choisissent d’externaliser leur hébergement sur un fournisseur d’hôte web.</span><span class="sxs-lookup"><span data-stu-id="d3f87-186">While it is possible to host ASP.NET websites on your personal computer or on servers within your company, many businesses and individuals choose to outsource their hosting to a web host provider.</span></span>

<span data-ttu-id="d3f87-187">Cette série de didacticiels examine les étapes pour déployer une application ASP.NET sur un fournisseur d’hôte web, exploration des défis courants.</span><span class="sxs-lookup"><span data-stu-id="d3f87-187">This tutorial series examines the steps for deploying an ASP.NET application to a web host provider, exploring common challenges.</span></span> <span data-ttu-id="d3f87-188">Ce didacticiel proposé une vue d’ensemble du processus de déploiement d’ASP.NET et a donné des conseils pour trouver un fournisseur d’hôte web approprié.</span><span class="sxs-lookup"><span data-stu-id="d3f87-188">This tutorial offered a high-level overview of the ASP.NET deployment process and gave tips for finding a suitable web host provider.</span></span> <span data-ttu-id="d3f87-189">Le didacticiel suivant examine liées à ASP.NET fichiers qui doivent être copiés vers l’environnement de production lors du déploiement de votre site Web.</span><span class="sxs-lookup"><span data-stu-id="d3f87-189">The next tutorial looks at what ASP.NET-related files need to be copied to the production environment when deploying your website.</span></span>

<span data-ttu-id="d3f87-190">Bonne programmation !</span><span class="sxs-lookup"><span data-stu-id="d3f87-190">Happy Programming!</span></span>

### <a name="special-thanks-to"></a><span data-ttu-id="d3f87-191">Remerciements particuliers à...</span><span class="sxs-lookup"><span data-stu-id="d3f87-191">Special Thanks To...</span></span>

<span data-ttu-id="d3f87-192">Cette série de didacticiels a été révisée par plusieurs réviseurs utiles.</span><span class="sxs-lookup"><span data-stu-id="d3f87-192">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="d3f87-193">Entraîner un réviseur pour ce didacticiel a été Teresa Murphy.</span><span class="sxs-lookup"><span data-stu-id="d3f87-193">Lead reviewer for this tutorial was Teresa Murphy.</span></span> <span data-ttu-id="d3f87-194">Qui souhaitent consulter mes prochains articles MSDN ?</span><span class="sxs-lookup"><span data-stu-id="d3f87-194">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="d3f87-195">Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).</span><span class="sxs-lookup"><span data-stu-id="d3f87-195">If so, drop me a line at [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3f87-196">[Précédent](users-and-roles-on-the-production-website-cs.md)
> [Suivant](determining-what-files-need-to-be-deployed-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d3f87-196">[Previous](users-and-roles-on-the-production-website-cs.md)
[Next](determining-what-files-need-to-be-deployed-vb.md)</span></span>