---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notes de sortie pour ASP.NET et Web Tools 2013.1 pour Visual Studio 2012 ( Microsoft Docs
author: rick-anderson
description: Ce document décrit la sortie de ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d4aced4e77a150d52358c2d2513ff79e6594a9de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543572"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="a6e9a-103">ASP.NET et Web Tools 2013.1 pour Visual Studio 2012 - Notes de publication</span><span class="sxs-lookup"><span data-stu-id="a6e9a-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<span data-ttu-id="a6e9a-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a6e9a-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a6e9a-105">Ce document décrit la sortie de ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

## <a name="contents"></a><span data-ttu-id="a6e9a-106">Contents</span><span class="sxs-lookup"><span data-stu-id="a6e9a-106">Contents</span></span>

- [<span data-ttu-id="a6e9a-107">Notes d’installation</span><span class="sxs-lookup"><span data-stu-id="a6e9a-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="a6e9a-108">Exigences logicielles</span><span class="sxs-lookup"><span data-stu-id="a6e9a-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="a6e9a-109">Nouvelles fonctionnalités dans ASP.NET et Web Tools 2013.1 pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="a6e9a-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="a6e9a-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="a6e9a-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="a6e9a-111">Modèles</span><span class="sxs-lookup"><span data-stu-id="a6e9a-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="a6e9a-112">modèle MVC 5 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a6e9a-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="a6e9a-113">modèle web API 2 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a6e9a-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="a6e9a-114">Modèles d’objets</span><span class="sxs-lookup"><span data-stu-id="a6e9a-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="a6e9a-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a6e9a-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="a6e9a-116">ASP.NET échafaudage</span><span class="sxs-lookup"><span data-stu-id="a6e9a-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="a6e9a-117">Éditeur de rasoir</span><span class="sxs-lookup"><span data-stu-id="a6e9a-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="a6e9a-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="a6e9a-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="a6e9a-119">Problèmes connus et changements de rupture</span><span class="sxs-lookup"><span data-stu-id="a6e9a-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="a6e9a-120">ASP.NET échafaudage</span><span class="sxs-lookup"><span data-stu-id="a6e9a-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="a6e9a-121">MVC et Web API Échafaudage - HTTP 404, erreur non trouvée</span><span class="sxs-lookup"><span data-stu-id="a6e9a-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="a6e9a-122">Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un article échafaudé</span><span class="sxs-lookup"><span data-stu-id="a6e9a-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="a6e9a-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="a6e9a-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="a6e9a-124">Affichage du fichier cshtml avec Browse With ou F5 provoque une erreur de serveur</span><span class="sxs-lookup"><span data-stu-id="a6e9a-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="a6e9a-125">Url Rewrite et Tilde(MD)</span><span class="sxs-lookup"><span data-stu-id="a6e9a-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="a6e9a-126">Modèles</span><span class="sxs-lookup"><span data-stu-id="a6e9a-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="a6e9a-127">Notes d'installation</span><span class="sxs-lookup"><span data-stu-id="a6e9a-127">Installation Notes</span></span>

<span data-ttu-id="a6e9a-128">[Installer](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="a6e9a-129">Configuration logicielle</span><span class="sxs-lookup"><span data-stu-id="a6e9a-129">Software Requirements</span></span>

<span data-ttu-id="a6e9a-130">Vous devez avoir Visual Studio 2012 ou Visual Studio Express 2012 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="a6e9a-131">Nouvelles fonctionnalités dans ASP.NET et Web Tools 2013.1 pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="a6e9a-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="a6e9a-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="a6e9a-132">Bootstrap</span></span>

<span data-ttu-id="a6e9a-133">Lorsque vous échafaudez les contrôleurs et les vues MVC 5, le balisage pour les vues utilise [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="a6e9a-134">Modèles</span><span class="sxs-lookup"><span data-stu-id="a6e9a-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="a6e9a-135">modèle MVC 5 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a6e9a-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="a6e9a-136">Nous avons ajouté un nouveau modèle MVC 5.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="a6e9a-137">Il fait référence aux derniers paquets MVC 5 NuGet, et vous pouvez utiliser des échafaudages pour ajouter des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="a6e9a-138">modèle web API 2 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a6e9a-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="a6e9a-139">Nous avons ajouté un nouveau modèle Web API 2.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="a6e9a-140">Il fait référence aux derniers paquets NuGet Web API 2, et vous pouvez utiliser des échafaudages pour ajouter des contrôleurs et des vues.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="a6e9a-141">Modèles d'élément</span><span class="sxs-lookup"><span data-stu-id="a6e9a-141">Item Templates</span></span>

<span data-ttu-id="a6e9a-142">Nous avons ajouté de nouveaux modèles d’éléments pour les vues MVC 5, les pages Web (Razor 3) et les contrôleurs Web API 2.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="a6e9a-143">Ils installent les paquets NuGet connexes au projet tout en ajoutant de nouveaux éléments.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="a6e9a-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a6e9a-144">Entity Framework 6</span></span>

<span data-ttu-id="a6e9a-145">Lorsque vous échafaudez un contrôleur MVC ou Web API à l’aide d’Entity Framework, nous utilisons le Cadre 6.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="a6e9a-146">Pour plus d’informations sur le cadre de l’entité, voir [l’histoire de la version cadre de l’entité](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="a6e9a-147">Vous pouvez également télécharger et installer les outils Du cadre d’entité 6 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="a6e9a-148">Voir le [cadre Get Entity](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="a6e9a-149">ASP.NET échafaudage</span><span class="sxs-lookup"><span data-stu-id="a6e9a-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="a6e9a-150">ASP.NET Scaffolding est un cadre de génération de code pour les applications Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="a6e9a-151">Il est facile d’ajouter du code de plaque chauffante à votre projet qui interagit avec un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="a6e9a-152">Dans les versions précédentes de Visual Studio, les échafaudages se limitaient à ASP.NET projets MVC.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="a6e9a-153">Avec cette mise à jour, vous pouvez maintenant utiliser des échafaudages pour n’importe quel projet ASP.NET, y compris les formulaires Web.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="a6e9a-154">Cette mise à jour ne prend pas en charge la génération de pages pour un projet Web Forms, mais vous pouvez toujours utiliser des échafaudages avec des formulaires Web en ajoutant des dépendances MVC au projet.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="a6e9a-155">La prise en charge de la génération de pages pour les formulaires Web sera ajoutée dans une prochaine mise à jour.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="a6e9a-156">Lors de l’utilisation d’échafaudages, nous nous assurons que toutes les dépendances requises sont installées dans le projet.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="a6e9a-157">Par exemple, si vous commencez par un projet ASP.NET Web Forms, puis utilisez des échafaudages pour ajouter un contrôleur d’API Web, les paquets et références NuGet requis sont ajoutés automatiquement à votre projet.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="a6e9a-158">Pour ajouter des échafaudages MVC à un projet Web Forms, ajoutez un **nouvel article échafaudé** et sélectionnez **les dépendances MVC 5** dans la fenêtre de dialogue.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="a6e9a-159">Il existe deux options pour l’échafaudage MVC; Minimal et complet.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="a6e9a-160">Si vous sélectionnez Minimal, seuls les paquets NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="a6e9a-161">Si vous sélectionnez l’option Complète, les dépendances minimales sont ajoutées, ainsi que les fichiers de contenu requis pour un projet MVC.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="a6e9a-162">Le support pour les contrôleurs async d’échafaudage utilise les nouvelles fonctionnalités async de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="a6e9a-163">Pour plus d’informations et de tutoriels, voir [ASP.NET Aperçu de l’échafaudage](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="a6e9a-164">Ces tutoriels montrent des échafaudages avec Visual Studio 2013, mais ils sont également applicables à ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="a6e9a-165">Éditeur de rasoir</span><span class="sxs-lookup"><span data-stu-id="a6e9a-165">Razor Editor</span></span>

<span data-ttu-id="a6e9a-166">Avec cette mise à jour, Visual Studio 2012 prend désormais en charge l’outillage/édition de Razor 3.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="a6e9a-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="a6e9a-167">NuGet 2.7</span></span>

<span data-ttu-id="a6e9a-168">NuGet 2.7 comprend un riche ensemble de nouvelles fonctionnalités qui sont décrites en détail à [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="a6e9a-169">Cette version de NuGet élimine la nécessité pour les utilisateurs de permettre explicitement à NuGet de restaurer les paquets manquants.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="a6e9a-170">Lors de l’installation de NuGet 2.7, les utilisateurs consentent implicitement à restaurer automatiquement les paquets manquants.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="a6e9a-171">Les utilisateurs peuvent se retirer explicitement de la restauration du paquet via les paramètres NuGet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="a6e9a-172">Ce changement simplifie le fonctionnement de la restauration des emballages.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="a6e9a-173">Problèmes connus et changements de rupture</span><span class="sxs-lookup"><span data-stu-id="a6e9a-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="a6e9a-174">ASP.NET échafaudage</span><span class="sxs-lookup"><span data-stu-id="a6e9a-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="a6e9a-175">MVC et Web API Échafaudage - HTTP 404, erreur non trouvée</span><span class="sxs-lookup"><span data-stu-id="a6e9a-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="a6e9a-176">Si vous rencontrez une erreur lors de l’ajout d’un élément échafaudé à un projet, il est possible que votre projet soit laissé dans un état incohérent.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="a6e9a-177">Certaines des modifications apportées à l’échafaudage seront annulées, mais d’autres changements, comme les paquets NuGet installés, ne seront pas annulés.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="a6e9a-178">Si les modifications de configuration de routage sont annulées, les utilisateurs recevront une erreur HTTP 404 lors de la navigation vers des éléments échafaudés.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="a6e9a-179">Pour corriger cette erreur pour MVC, ajoutez un nouvel article échafaudé et sélectionnez les dépendances MVC 5 (minimales ou complètes).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="a6e9a-180">Ce processus ajoutera toutes les modifications requises à votre projet.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="a6e9a-181">Pour corriger cette erreur pour l’API Web :</span><span class="sxs-lookup"><span data-stu-id="a6e9a-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="a6e9a-182">Ajoutez la classe WebApiConfig suivante à votre projet.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="a6e9a-183">Configurez WebApiConfig.Register\_dans la méthode De démarrage d’application dans Global.asax comme suit :</span><span class="sxs-lookup"><span data-stu-id="a6e9a-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="a6e9a-184">Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un article échafaudé</span><span class="sxs-lookup"><span data-stu-id="a6e9a-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="a6e9a-185">Si Visual Studio Express 2012 pour Le Web cesse de fonctionner après l’ajout d’un élément échafaudé avec Entity Framework (comme Web API 2 Controller avec des actions, en utilisant Entity Framework), il est possible que Visual Studio Express n’ait pas chargé l’image native d’un assemblage dépendant de System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="a6e9a-186">Pour corriger ce problème, configurez Visual Studio Express pour travailler avec l’image MSIL de System.Web.Extensions :</span><span class="sxs-lookup"><span data-stu-id="a6e9a-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="a6e9a-187">Ouvrez l’invite de commande en mode Administrateur.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="a6e9a-188">Rendez-vous à %ProgramFiles% -Microsoft Visual Studio 11.0-Common7-IDE ou %ProgramFiles(x86)% -Microsoft Visual Studio 11.0-Common7-IDE (pour 64 bits Windows).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="a6e9a-189">Ouvrez VWDExpress.exe.config dans un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="a6e9a-190">Ajoutez la ligne &lt;suivante&gt;/&lt;sous&gt; l’élément de temps d’exécution de configuration :</span><span class="sxs-lookup"><span data-stu-id="a6e9a-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="a6e9a-191">Redémarrez Visual Studio Express 2012 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="a6e9a-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="a6e9a-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a><span data-ttu-id="a6e9a-193">Affichage du fichier cshtml avec Browse With ou F5 provoque une erreur de serveur</span><span class="sxs-lookup"><span data-stu-id="a6e9a-193">Viewing cshtml file with Browse With or F5 causes a server error</span></span>

<span data-ttu-id="a6e9a-194">Lorsque vous créez un projet MVC 5 dans Visual Studio 2012 (ou ou ouvrez dans Visual Studio 2012 un projet MVC 5 qui a été créé dans Visual Studio 2013) et tentez de visualiser un fichier cshtml en utilisant Browse With ou F5, vous recevrez une erreur qui indique **l’erreur du serveur dans l’application ' /.**</span><span class="sxs-lookup"><span data-stu-id="a6e9a-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="a6e9a-195">Le serveur tente de naviguer vers`http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="a6e9a-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="a6e9a-196">Pour résoudre ce problème, modifiez le paramètre **Start Action** dans votre projet en **Page Spécifique**.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="a6e9a-197">Vous n’avez pas besoin de fournir une valeur pour la page.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="a6e9a-198">Après avoir apporté ce changement, la sélection de F5 navigue jusqu’à la racine de votre application (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="a6e9a-199">Ce comportement n’est pas le même que le comportement des projets MVC 5 dans Visual Studio 2013, où le paramètre De la **Page actuelle** lance la page ouverte.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="a6e9a-200">Url Rewrite et Tilde(MD)</span><span class="sxs-lookup"><span data-stu-id="a6e9a-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="a6e9a-201">Après la mise à niveau vers ASP.NET Razor 3 ou ASP.NET MVC 5, la notation tilde (MD) peut ne plus fonctionner correctement si vous utilisez des réécritures d’URL.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="a6e9a-202">La réécriture de l’URL affecte la notation &lt;tilde (MD) dans les éléments HTML tels que&gt;A/ &lt;, SCRIPT/&gt;, &lt;LINK/&gt;, et par conséquent le tilde ne cartes plus à l’annuaire racine.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="a6e9a-203">Par exemple, si vous réécrivez les demandes **de asp.net/content** &lt;à **asp.net,** l’attribut href&gt; dans A href"/content/"/ se résout à **/contenu/contenu/** au lieu de **/**.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="a6e9a-204">Pour supprimer ce changement, vous pouvez définir le contexte **iÉlRécrit DE l’IIS\_** à faux dans chaque page Web ou dans **l’application\_BeginRequest** dans Global.asax.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="a6e9a-205">Modèles</span><span class="sxs-lookup"><span data-stu-id="a6e9a-205">Templates</span></span>

<span data-ttu-id="a6e9a-206">Lorsque vous créez ASP.NET projets MVC avec Visual Studio 2012 sur Windows 8.1 ou Windows Server 2012 R2, Visual Studio affiche un message d’erreur qui indique "Configuring Web [url] pour ASP.NET 4.5 a échoué."</span><span class="sxs-lookup"><span data-stu-id="a6e9a-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![erreur de configuration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="a6e9a-208">Vous voyez cette erreur car Visual Studio 2012 ne permet pas la ASP.NET fonction 4.5 lorsqu’elle est installée sur ces versions de Windows.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="a6e9a-209">Pour activer ASP.NET 4.5, effectuez les étapes décrites dans les [fonctionnalités De Turn Windows sur ou en dehors](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="a6e9a-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![activer ou désactiver les fonctionnalités Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="a6e9a-211">Alternativement, vous pouvez activer ASP.NET 4.5 à travers la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="a6e9a-212">Ouvrez l’invite de commande en mode Administrateur.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="a6e9a-213">Exécutez la commande suivante pour activer ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a6e9a-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
