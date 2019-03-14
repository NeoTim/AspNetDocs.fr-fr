---
title: Introduction aux autorisations dans ASP.NET Core
author: rick-anderson
description: Découvrez les principes de base d’autorisation et de fonctionne de l’autorisation dans les applications ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046366"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="a4c0e-103">Introduction aux autorisations dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4c0e-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="a4c0e-104">Le terme Autorisation désigne le processus qui détermine ce qu’un utilisateur est en mesure d’effectuer.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="a4c0e-105">Par exemple, un utilisateur administratif est autorisé à créer une bibliothèque de documents, ajoutez les documents, modifier des documents et les supprimer.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="a4c0e-106">Un utilisateur non-administrateur, utilisant la bibliothèque est uniquement autorisé à lire les documents.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="a4c0e-107">L’autorisation est orthogonale et indépendante de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="a4c0e-108">Toutefois, l’autorisation nécessite un mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="a4c0e-109">L’authentification est le processus d’évaluation qui est un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="a4c0e-110">L’authentification peut créer une ou plusieurs identités pour l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="a4c0e-111">Types d’autorisation</span><span class="sxs-lookup"><span data-stu-id="a4c0e-111">Authorization types</span></span>

<span data-ttu-id="a4c0e-112">L'autorisation de ASP.NET Core fournit un simple [rôle](xref:security/authorization/roles) déclaratif et un [modèle enrichie basée sur la stratégie](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="a4c0e-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="a4c0e-113">L’autorisation est exprimée dans les exigences et les gestionnaires évaluent les revendications d’un utilisateur par rapport aux exigences.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="a4c0e-114">Les vérifications impératives peuvent reposer sur des stratégies simples ou des stratégies qui évaluent l’identité de l’utilisateur et les propriétés de la ressource à laquelle l’utilisateur tente d’accéder.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="a4c0e-115">Espaces de noms</span><span class="sxs-lookup"><span data-stu-id="a4c0e-115">Namespaces</span></span>

<span data-ttu-id="a4c0e-116">Les composants d’autorisation, y compris les attributs `AuthorizeAttribute` et `AllowAnonymousAttribute` se trouvent dans l'espace de noms `Microsoft.AspNetCore.Authorization`.</span><span class="sxs-lookup"><span data-stu-id="a4c0e-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="a4c0e-117">Consultez la documentation sur [autorisation simple](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="a4c0e-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
