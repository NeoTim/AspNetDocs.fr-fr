---
title: Articles basés sur les projets ASP.NET Core créés avec les comptes d’utilisateur individuels
author: rick-anderson
description: Découvrir des articles basés sur les projets ASP.NET Core créés avec les comptes d’utilisateur individuels.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: c73365eafaf2c38ef02c3c83ccf5ced4264f7dc0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033836"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="4e693-103">Articles basés sur les projets ASP.NET Core créés avec les comptes d’utilisateur individuels</span><span class="sxs-lookup"><span data-stu-id="4e693-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="4e693-104">ASP.NET Core Identity est inclus dans les modèles de projet dans Visual Studio avec l’option « Comptes d’utilisateur individuels ».</span><span class="sxs-lookup"><span data-stu-id="4e693-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="4e693-105">Les modèles d’authentification sont disponibles dans l’interface CLI .NET Core avec `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="4e693-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="4e693-106">Consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/5833) pour l’authentification d’API web.</span><span class="sxs-lookup"><span data-stu-id="4e693-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="4e693-107">Aucune authentification</span><span class="sxs-lookup"><span data-stu-id="4e693-107">No Authentication</span></span>

<span data-ttu-id="4e693-108">L’authentification est spécifiée dans l’interface CLI .NET Core avec la `-au` option.</span><span class="sxs-lookup"><span data-stu-id="4e693-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="4e693-109">Dans Visual Studio, le **modifier l’authentification** boîte de dialogue est disponible pour les nouvelles applications web.</span><span class="sxs-lookup"><span data-stu-id="4e693-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="4e693-110">La valeur par défaut pour les nouvelles applications web dans Visual Studio est **aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="4e693-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="4e693-111">Projets créés avec aucune authentification :</span><span class="sxs-lookup"><span data-stu-id="4e693-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="4e693-112">Ne contiennent pas les pages web et l’interface utilisateur pour se connecter et se déconnecter.</span><span class="sxs-lookup"><span data-stu-id="4e693-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="4e693-113">Ne contiennent pas le code d’authentification.</span><span class="sxs-lookup"><span data-stu-id="4e693-113">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="4e693-114">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="4e693-114">Windows Authentication</span></span>

<span data-ttu-id="4e693-115">L’authentification Windows est spécifiée pour les nouvelles applications web dans l’interface CLI .NET Core avec la `-au Windows` option.</span><span class="sxs-lookup"><span data-stu-id="4e693-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="4e693-116">Dans Visual Studio, le **modifier l’authentification** boîte de dialogue fournit le **l’authentification Windows** options.</span><span class="sxs-lookup"><span data-stu-id="4e693-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="4e693-117">Si l’authentification Windows est sélectionnée, l’application est configurée pour utiliser le [module IIS de l’authentification Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="4e693-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="4e693-118">L’authentification Windows est conçue pour les sites web Intranet.</span><span class="sxs-lookup"><span data-stu-id="4e693-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e693-119">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4e693-119">Additional resources</span></span>

<span data-ttu-id="4e693-120">Les articles suivants vous montrent comment utiliser le code généré dans les modèles ASP.NET Core qui utilisent des comptes d’utilisateur individuels :</span><span class="sxs-lookup"><span data-stu-id="4e693-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="4e693-121">Authentification à deux facteurs avec SMS</span><span class="sxs-lookup"><span data-stu-id="4e693-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="4e693-122">Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="4e693-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="4e693-123">Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation</span><span class="sxs-lookup"><span data-stu-id="4e693-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
