---
uid: webhooks/diagnostics/logging
title: ASP.NET WebHooks enregistrant Microsoft Docs
author: rick-anderson
description: Comment faire la connexion dans ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 350732acbd3a73bddb8f8b20dcd50c225d89be82
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675344"
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET WebHooks enregistrant

Microsoft ASP.NET WebHooks utilise l’enregistrement comme un moyen de signaler les problèmes et les problèmes. Par défaut, les journaux sont écrits à l’aide [de System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) où ils peuvent être consommés à l’aide [de Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) comme n’importe quel autre flux de journaux.

Lors du déploiement de votre application Web en tant qu’application Web Azure, les journaux sont automatiquement repris et peuvent être gérés avec n’importe quel autre [site System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) enregistrant. Pour plus de détails, veuillez consulter [la connexion de diagnostics Enable pour les applications Web dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

En outre, les journaux peuvent être obtenus directement à partir de l’intérieur Visual Studio comme décrit dans [Troubleshoot une application web dans Azure App Service en utilisant Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Rediriger les journaux

Au lieu d’écrire des journaux à [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), il est possible de fournir une mise en œuvre de connexion alternative qui peut se connecter directement à un gestionnaire de journal tels que [Log4Net](http://logging.apache.org/log4net/) et [NLog](http://nlog-project.org/). Il suffit de fournir une mise en œuvre de [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) et l’enregistrer avec un moteur d’injection de dépendance de votre choix et il sera ramassé par Microsoft ASP.NET WebHooks. S’il vous plaît voir [l’injection de dépendance dans ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) pour plus de détails.
