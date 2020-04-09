---
uid: webhooks/diagnostics/debugging
title: ASP.NET WebHooks débogage (fr) Microsoft Docs
author: rick-anderson
description: Comment déboiffer ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 2f1f8196478e7025a0467acb945d9ed36c8fd0ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675371"
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET WebHooks débogage

## <a name="debugging-in-azure"></a>Débogage à Azure

Pour déboguer votre application Web en cours d’exécution à Azure, veuillez consulter le tutoriel [Troubleshoot une application web dans Azure App Service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Débogage avec Source et Symboles

En plus de débogage de votre propre code, il est possible de déboguer directement dans Microsoft ASP.NET WebHooks, et en fait tout de .NET. Cela fonctionne indépendamment du fait que vous déboillez localement ou à distance. Tout d’abord, configurer Visual Studio pour trouver la source et les symboles en allant à **Debug,** puis **Options et Paramètres**. Définissez les options comme celle-ci :

![Options et Paramètres](_static/SourceSymbols.png)

Ensuite, ajoutez un lien vers [symbolsource.org](http://symbolsource.org) pour télécharger la source et les symboles. Rendez-vous sur l’onglet **Symboles** du menu ci-dessus et ajoutez ce qui suit comme emplacement de symbole :

```
http://srv.symbolsource.org/pdb/Public
```

En outre, assurez-vous que le répertoire cache a un nom court; sinon les noms de fichiers peuvent devenir trop longs, ce qui fera que les symboles ne se chargent pas. Un échantillon de chemin est :

```
C:\SymCache
```

Les paramètres doivent ressembler à ceci :

![Exemple de localisation de fichier de symbole d’options](_static/SymSource.png)
