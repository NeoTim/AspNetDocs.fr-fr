---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Authentifier les utilisateurs avec l’authentification windows (VB) Microsoft Docs
author: rick-anderson
description: Découvrez comment utiliser l’authentification Windows dans le contexte d’une application MVC. Vous apprenez à activer l’authentification de Windows dans le web co de votre application...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 446dcc338f61e1f76478c1085773e7ad089c73f4
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540595"
---
# <a name="authenticating-users-with-windows-authentication-vb"></a>Authentification des utilisateurs avec l’authentification Windows (VB)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment utiliser l’authentification Windows dans le contexte d’une application MVC. Vous apprenez comment activer l’authentification de Windows dans le fichier de configuration Web de votre application et comment configurer l’authentification avec IIS. Enfin, vous apprenez à utiliser l’attribut [Autoriser] pour limiter l’accès aux actions de contrôleur à des utilisateurs ou groupes Windows particuliers.

Le but de ce tutoriel est d’expliquer comment vous pouvez profiter des fonctionnalités de sécurité intégrées dans les services d’information Internet pour protéger les vues de votre MVC. Vous apprenez à autoriser les actions de contrôleur à être invoquées uniquement par des utilisateurs ou utilisateurs de Windows particuliers qui sont membres de groupes Windows particuliers.

L’utilisation de l’authentification Windows est logique lorsque vous construisez un site Web interne de l’entreprise (un site intranet) et que vous souhaitez que vos utilisateurs puissent utiliser leurs noms d’utilisateur et mots de passe Windows standard lors de l’accès au site Web. Si vous construisez un site Web orienté vers l’extérieur (un site Internet) envisagez d’utiliser l’authentification des formulaires à la place.

#### <a name="enabling-windows-authentication"></a>Permettre l’authentification de Windows

Lorsque vous créez une nouvelle application ASP.NET MVC, l’authentification Windows n’est pas activée par défaut. L’authentification des formulaires est le type d’authentification par défaut activé pour les applications MVC. Vous devez activer l’authentification Windows en modifiant le fichier de configuration Web (web.config) de votre application MVC. Trouvez &lt;la&gt; section d’authentification et modifiez-la pour utiliser Windows au lieu de l’authentification des formes comme celle-ci :

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Lorsque vous activez l’authentification de Windows, votre serveur Web devient responsable de l’authentification des utilisateurs. En règle générale, il existe deux types différents de serveurs Web que vous utilisez lors de la création et le déploiement d’une application MVC ASP.NET.

Tout d’abord, lors du développement d’une application MVC, vous utilisez le serveur Web de développement ASP.NET inclus avec Visual Studio. Par défaut, le serveur Web de développement ASP.NET exécute toutes les pages dans le cadre du compte Windows actuel (quel que soit le compte que vous avez utilisé pour vous connecter à Windows).

Le serveur Web de développement ASP.NET prend également en charge l’authentification NTLM. Vous pouvez activer l’authentification NTLM en cliquant à droite sur le nom de votre projet dans la fenêtre Solution Explorer et en sélectionnant les propriétés. Ensuite, sélectionnez l’onglet Web et vérifiez la case à cocher NTLM (voir la figure 1).

**Figure 1 - Permettre l’authentification NTLM pour le serveur Web de développement ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Pour une application web de production, vous utilisez IIS comme serveur web. L’IIS prend en charge plusieurs types d’authentification, notamment :

- Authentification de base - Définie dans le cadre du protocole HTTP 1.0. Envoie des noms d’utilisateur et des mots de passe en texte clair (Base64 codé) sur Internet. - Digest Authentification - Envoie un hachage d’un mot de passe, au lieu du mot de passe lui-même, à travers l’Internet. - Authentification intégrée de Windows (NTLM) - Le meilleur type d’authentification à utiliser dans les environnements intranet à l’aide de fenêtres. - Authentification des certificats - Permet l’authentification à l’aide d’un certificat côté client. Le certificat est par carte d’un compte utilisateur Windows.

> [!NOTE] 
> 
> Pour un aperçu plus détaillé de ces [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx)différents types d’authentification, voir .

Vous pouvez utiliser Internet Information Services Manager pour activer un type particulier d’authentification. Sachez que tous les types d’authentification ne sont pas disponibles dans le cas de chaque système d’exploitation. En outre, si vous utilisez IIS 7.0 avec Windows Vista, vous devrez activer les différents types d’authentification Windows avant qu’ils apparaissent dans le gestionnaire des services d’information Internet. **Panneau de contrôle ouvert, programmes, programmes et fonctionnalités, Activez ou éteignez**les fonctionnalités de Windows et élargissez le nœud des services d’information Internet (voir la figure 2).

**Figure 2 - Permettant les fonctionnalités de Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

À l’aide de services d’information Internet, vous pouvez activer ou désactiver différents types d’authentification. Par exemple, la figure 3 illustre la désactivation de l’authentification anonyme et l’activation de l’authentification intégrée des fenêtres (NTLM) lors de l’utilisation de l’IIS 7.0.

**Figure 3 - Permettre l’authentification intégrée de Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autoriser les utilisateurs et les groupes Windows

Après avoir permis l’authentification&gt; Windows, vous pouvez utiliser l’attribut Autoriser pour contrôler l’accès &lt;aux contrôleurs ou aux actions de contrôleur. Cet attribut peut être appliqué à un contrôleur MVC entier ou à une action de contrôleur particulier.

Par exemple, le contrôleur à domicile dans la liste 1 expose trois actions nommées Index(), CompanySecrets() et StephenSecrets(). N’importe qui peut invoquer l’action Index() . Toutefois, seuls les membres du groupe windows Local Managers peuvent invoquer l’action CompanySecrets(). Enfin, seul l’utilisateur du domaine Windows nommé Stephen (dans le domaine de Redmond) peut invoquer l’action StephenSecrets().

**Liste 1 - Controllers-HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> En raison du contrôle des comptes utilisateur Windows (UAC), lorsque vous travaillez avec Windows Vista ou Windows Server 2008, le groupe d’administrateurs locaux se comportera différemment des autres groupes. &lt;L’attribut&gt; d’autorisation ne reconnaîtra pas correctement un membre du groupe d’administrateurs locaux à moins que vous ne modifiiez les paramètres UAC de votre ordinateur.

Exactement ce qui se passe lorsque vous tentez d’invoquer une action de contrôleur sans être les bonnes autorisations dépend du type d’authentification activée. Par défaut, lorsque vous utilisez le serveur de développement ASP.NET, vous obtenez simplement une page blanche. La page est servie avec un **401 Non Autorisé** STATUT de réponse HTTP.

Si, d’autre part, vous utilisez IIS avec l’authentification anonyme désactivée et l’authentification de base activée, alors vous continuez à obtenir un dialogue de connexion invite chaque fois que vous demandez la page protégée (voir la figure 4).

**Figure 4 - Dialogue de connexion d’authentification de base**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Récapitulatif

Ce tutoriel a expliqué comment vous pouvez utiliser l’authentification Windows dans le cadre d’une application MVC ASP.NET. Vous avez appris à activer l’authentification de Windows dans le fichier de configuration Web de votre application et comment configurer l’authentification avec IIS. Enfin, vous avez appris &lt;à&gt; utiliser l’attribut Autorisation pour restreindre l’accès aux actions de contrôleur à des utilisateurs ou groupes Windows particuliers.

> [!div class="step-by-step"]
> [Suivant précédent](authenticating-users-with-forms-authentication-vb.md)
> [Next](preventing-javascript-injection-attacks-vb.md)
