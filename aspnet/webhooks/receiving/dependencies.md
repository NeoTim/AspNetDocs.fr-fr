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
# <a name="aspnet-webhooks-receiver-dependencies"></a>dépendances ASP.NET de récepteur WebHooks

Microsoft ASP.NET WebHooks est conçu avec l’injection de dépendance à l’esprit. La plupart des dépendances dans le système peuvent être remplacées par des implémentations alternatives à l’aide d’un moteur d’injection de dépendance.

Voir [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) pour une liste de dépendances récepteur. Si aucune dépendance n’a été enregistrée, une implémentation par défaut est utilisée. Consultez [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) pour une liste d’implémentations par défaut.
