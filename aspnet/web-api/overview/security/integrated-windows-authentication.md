---
uid: web-api/overview/security/integrated-windows-authentication
title: L’authentification Windows intégrée | Microsoft Docs
author: MikeWasson
description: Décrit l’utilisation de l’authentification Windows intégrée dans l’API Web ASP.NET.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: ce845eb6c914321736d77e989f10344eb7596eba
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59416829"
---
# <a name="integrated-windows-authentication"></a>Authentification Windows intégrée

par [Mike Wasson](https://github.com/MikeWasson)

L’authentification Windows intégrée permet aux utilisateurs de se connecter avec leurs informations d’identification Windows, à l’aide de Kerberos ou NTLM. Le client envoie des informations d’identification dans l’en-tête d’autorisation. L’authentification Windows est idéale pour un environnement intranet. Pour plus d’informations, consultez [Authentification Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Avantages | Inconvénients |
| --- | --- |
| -Intégrée à IIS. -N’envoie pas des informations d’identification de l’utilisateur dans la demande. -Si l’ordinateur client appartient au domaine (par exemple, les applications intranet), l’utilisateur n’a pas besoin entrer les informations d’identification. | -N’est pas recommandé pour les applications Internet. -Nécessite la prise en charge Kerberos ou NTLM dans le client. -Le client doit être dans le domaine Active Directory. |

> [!NOTE]
> Si votre application est hébergée sur Azure et que vous avez un domaine Active Directory sur site, envisagez de fédérer votre AD local avec Azure Active Directory. De cette façon, les utilisateurs peuvent se connecter avec leurs informations d’identification en local, mais l’authentification est effectuée par Azure AD. Pour plus d’informations, consultez [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Pour créer une application qui utilise l’authentification Windows intégrée, sélectionnez le modèle « Application Intranet » dans l’Assistant de projet MVC 4. Ce modèle de projet place le paramètre suivant dans le fichier Web.config :

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Du côté client, l’authentification Windows intégrée fonctionne avec n’importe quel navigateur prenant en charge la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) schéma d’authentification, qui inclut la plupart des principaux navigateurs. Pour les applications clientes .NET, le **HttpClient** classe prend en charge l’authentification Windows :

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

L’authentification Windows est vulnérable aux attaques par falsification de requête intersites. Consultez [empêcher les falsifications de requête intersites (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).
