---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Authentification des utilisateurs avec l’authentification Windows (VB) | Microsoft Docs
author: microsoft
description: Découvrez comment utiliser l’authentification Windows dans le contexte d’une application MVC. Vous allez apprendre à activer l’authentification Windows au sein de la quantité de co de votre application web...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: aa64b1f9ef6461a81611ca066310dca2d545baa3
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65126835"
---
# <a name="authenticating-users-with-windows-authentication-vb"></a>Authentification des utilisateurs avec l’authentification Windows (VB)

by [Microsoft](https://github.com/microsoft)

> Découvrez comment utiliser l’authentification Windows dans le contexte d’une application MVC. Vous découvrez comment activer l’authentification Windows dans le fichier de configuration de votre application web et comment configurer l’authentification avec IIS. Enfin, vous allez apprendre à utiliser l’attribut [Authorize] pour restreindre l’accès aux actions de contrôleur pour les utilisateurs Windows particuliers ou des groupes.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez tirer parti de la sécurité, fonctionnalités intégrées à Internet Information Services pour le mot de passe protègent les vues dans vos applications MVC. Vous allez apprendre à autoriser des actions de contrôleur devant être appelé uniquement par les utilisateurs Windows particuliers ou les utilisateurs qui sont membres de groupes Windows spécifiques.

À l’aide de l’authentification Windows judicieux lorsque vous créez un site Web d’entreprise interne (un site intranet) et vous souhaitez que vos utilisateurs à être en mesure d’utiliser leurs noms d’utilisateur Windows standards et les mots de passe lors de l’accès du site Web. Si vous générez un vers l’extérieur accessible sur le site Web (un site Web Internet) envisagez plutôt d’utiliser l’authentification par formulaire.

#### <a name="enabling-windows-authentication"></a>Activation de l’authentification Windows

Lorsque vous créez une application ASP.NET MVC, l’authentification Windows n’est pas activée par défaut. L’authentification par formulaire est le type d’authentification par défaut activé pour les applications MVC. Vous devez activer l’authentification Windows en modifiant le fichier de configuration (web.config) de votre application MVC web. Rechercher la &lt;authentification&gt; section et modifiez-le pour utiliser Windows au lieu de l’authentification par formulaire comme suit :

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Lorsque vous activez l’authentification Windows, votre serveur web devient responsable de l’authentification des utilisateurs. En règle générale, il existe deux types de serveurs web que vous utilisez lorsque vous créez et déployez une application ASP.NET MVC.

Tout d’abord, lorsqu’il développe une application MVC, vous utilisez le serveur Web de développement ASP.NET fourni avec Visual Studio. Par défaut, le serveur Web de développement ASP.NET exécute toutes les pages dans le contexte du compte Windows actuel (quelle que soit compte utilisées pour vous connecter à Windows).

Le serveur Web de développement ASP.NET prend également en charge l’authentification NTLM. Vous pouvez activer l’authentification NTLM en cliquant sur le nom de votre projet dans la fenêtre Explorateur de solutions et en sélectionnant Propriétés. Ensuite, sélectionnez l’onglet Web et activez la case à cocher NTLM (voir Figure 1).

**Figure 1 : activation de l’authentification NTLM pour le serveur Web de développement ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Pour une application web de production, la part, vous utilisez IIS comme votre serveur web. IIS prend en charge plusieurs types d’authentification, notamment :

- Authentification de base défini dans le cadre du protocole HTTP 1.0. Envoie les noms d’utilisateur et mots de passe en texte clair (codé en Base64) via Internet. -L’authentification digest : envoie un hachage de mot de passe, au lieu du mot de passe lui-même, via internet. -Authentification intégrée Windows (NTLM) – le meilleur type d’authentification à utiliser dans les environnements intranet à l’aide de windows. -Certificat d’authentification : authentification permet à l’aide d’un certificat côté client. Le certificat est mappé à un compte d’utilisateur Windows.

> [!NOTE] 
> 
> Pour obtenir une présentation plus détaillée de ces différents types d’authentification, consultez [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).

Vous pouvez utiliser le Gestionnaire des Services Internet pour activer un type particulier d’authentification. N’oubliez pas que tous les types d’authentification ne sont pas disponibles dans le cas de tous les systèmes d’exploitation. En outre, si vous utilisez IIS 7.0 avec Windows Vista, vous devez activer les différents types d’authentification de Windows avant qu’elles apparaissent dans le Gestionnaire des Services Internet. Ouvrez **le panneau de configuration, des programmes, des programmes et des fonctionnalités, ou désactiver des fonctionnalités Windows activer**, puis développez le nœud Internet Information Services (voir Figure 2).

**Figure 2 – fonctionnalités d’activation Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

À l’aide d’Internet Information Services, vous pouvez activer ou désactiver les différents types d’authentification. Par exemple, la Figure 3 illustre la désactivation de l’authentification anonyme et l’activation de l’authentification intégrée Windows (NTLM) lors de l’utilisation d’IIS 7.0.

**Figure 3 : activation de l’authentification Windows intégrée**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autoriser Windows utilisateurs et groupes

Après avoir activé l’authentification Windows, vous pouvez utiliser la &lt;Authorize&gt; attribut pour contrôler l’accès aux contrôleurs ou les actions de contrôleur. Cet attribut peut être appliqué à un contrôleur MVC entière ou une action de contrôleur particulier.

Par exemple, le contrôleur Home dans le Listing 1 expose trois actions nommées Index(), CompanySecrets() et StephenSecrets(). Tout le monde peut appeler l’action Index(). Toutefois, seuls les membres du groupe Windows local gestionnaires peuvent appeler l’action de CompanySecrets(). Enfin, seul l’utilisateur de domaine Windows nommé Stephen (dans le domaine Redmond) permettre appeler l’action de StephenSecrets().

**Liste 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> En raison de Windows compte contrôle utilisateur (UAC), lorsque vous travaillez avec Windows Vista ou Windows Server 2008, le groupe Administrateurs local se comporte différemment des autres groupes. Le &lt;Authorize&gt; attribut ne reconnaît correctement un membre du groupe Administrateurs local, sauf si vous modifiez les paramètres de compte d’utilisateur de votre ordinateur.

Exactement ce qui se passe lorsque vous tentez d’appeler une action de contrôleur sans être les autorisations appropriées varie selon le type de l’authentification est activée. Par défaut, lorsque vous utilisez le serveur de développement ASP.NET, vous obtenez simplement une page vierge. La page est servie avec un **401 non autorisé** état de la réponse HTTP.

Si, en revanche, vous utilisez IIS avec l’authentification anonyme est désactivée et l’authentification de base est activée, vous continuez à recevoir une invite de boîte de dialogue de connexion chaque fois que vous demandez la page protégée (voir Figure 4).

**Figure 4 – boîte de dialogue de connexion d’authentification de base**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Récapitulatif

Ce didacticiel vous a expliqué comment vous pouvez utiliser l’authentification Windows dans le contexte d’une application ASP.NET MVC. Vous avez appris comment activer l’authentification Windows dans le fichier de configuration de votre application web et comment configurer l’authentification avec IIS. Enfin, vous avez appris à utiliser le &lt;Authorize&gt; attribut pour restreindre l’accès aux actions de contrôleur pour les utilisateurs Windows particuliers ou des groupes.

> [!div class="step-by-step"]
> [Précédent](authenticating-users-with-forms-authentication-vb.md)
> [Suivant](preventing-javascript-injection-attacks-vb.md)
