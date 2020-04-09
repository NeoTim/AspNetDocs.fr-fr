---
uid: webhooks/receiving/dependencies
title: ASP.NET dépendances du récepteur WebHooks (fr) Microsoft Docs
author: rick-anderson
description: Dépendances de récepteur et injection de dépendance dans ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: b50442b3d95512bc0db7583b93de3bbef2d4bb4a
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675202"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="58d85-103">dépendances ASP.NET de récepteur WebHooks</span><span class="sxs-lookup"><span data-stu-id="58d85-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="58d85-104">Microsoft ASP.NET WebHooks est conçu avec l’injection de dépendance à l’esprit.</span><span class="sxs-lookup"><span data-stu-id="58d85-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="58d85-105">La plupart des dépendances dans le système peuvent être remplacées par des implémentations alternatives à l’aide d’un moteur d’injection de dépendance.</span><span class="sxs-lookup"><span data-stu-id="58d85-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="58d85-106">Voir [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) pour une liste de dépendances récepteur.</span><span class="sxs-lookup"><span data-stu-id="58d85-106">See [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="58d85-107">Si aucune dépendance n’a été enregistrée, une implémentation par défaut est utilisée.</span><span class="sxs-lookup"><span data-stu-id="58d85-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="58d85-108">Consultez [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) pour une liste d’implémentations par défaut.</span><span class="sxs-lookup"><span data-stu-id="58d85-108">See [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
