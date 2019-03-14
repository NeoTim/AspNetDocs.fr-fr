---
ms.openlocfilehash: 733a41a76e289f8aed6ec2d246ed720e44808fec
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57182759"
---
L’appel à [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) configure les paramètres de schéma par défaut. Le [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) surcharger définit le [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) propriété. Le [AddAuthentication (Action&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) surcharge permet de configurer les options d’authentification, qui peuvent être utilisées pour définir des schémas d’authentification par défaut à des fins différentes. Les appels suivants à `AddAuthentication` remplacement configuré précédemment [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) propriétés.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) les méthodes d’extension qui inscrivent un gestionnaire d’authentification peuvent être appelées uniquement une fois par le schéma d’authentification. Il existe des surcharges qui permettent de configurer les propriétés de schéma, le nom du schéma d’et le nom complet.
