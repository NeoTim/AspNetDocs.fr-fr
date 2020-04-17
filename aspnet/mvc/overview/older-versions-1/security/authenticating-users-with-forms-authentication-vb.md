---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Authentifier les utilisateurs avec l’authentification des formulaires (VB) Microsoft Docs
author: rick-anderson
description: Apprenez à utiliser l’attribut [Autoriser] pour protéger les pages particulières de votre application MVC. Vous apprenez à utiliser l’administration du site Web trop ...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e3117af55db2effed20b6421c2322f1c265f1c7
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540816"
---
# <a name="authenticating-users-with-forms-authentication-vb"></a>Authentification des utilisateurs avec l’authentification par formulaire (VB)

par [Microsoft](https://github.com/microsoft)

> Apprenez à utiliser l’attribut [Autoriser] pour protéger les pages particulières de votre application MVC. Vous apprenez à utiliser l’outil d’administration de site Web pour créer et gérer les utilisateurs et les rôles. Vous apprenez également à configurer l’endroit où les informations de compte utilisateur et de rôle sont stockées.

Le but de ce tutoriel est d’expliquer comment vous pouvez utiliser l’authentification des formulaires pour protéger les vues de votre ASP.NET applications MVC. Vous apprenez à utiliser l’outil d’administration du site Web pour créer des utilisateurs et des rôles. Vous apprenez également comment empêcher les utilisateurs non autorisés d’invoquer des actions de contrôleur. Enfin, vous apprenez à configurer où les noms d’utilisateur et les mots de passe sont stockés.

#### <a name="using-the-web-site-administration-tool"></a>Utilisation de l’outil d’administration du site Web

Avant de faire autre chose, nous devrions commencer par créer des utilisateurs et des rôles. La façon la plus simple de créer de nouveaux utilisateurs et de nouveaux rôles est de profiter de l’outil d’administration du site Web Visual Studio 2008. Vous pouvez lancer cet outil en sélectionnant l’option de menu **Project, ASP.NET Configuration**. Alternativement, vous pouvez lancer l’outil d’administration du site Web en cliquant sur l’icône (un peu effrayante) du marteau frapper le monde qui apparaît en haut de la fenêtre Solution Explorer (voir la figure 1).

**Figure 1 - Lancement de l’outil d’administration du site Web**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Dans l’outil d’administration du site Web, vous créez de nouveaux utilisateurs et de nouveaux rôles en sélectionnant l’onglet Sécurité. Cliquez sur le lien **utilisateur Créer** pour créer un nouvel utilisateur nommé Stephen (voir la figure 2). Fournir à l’utilisateur Stephen n’importe quel mot de passe que vous voulez (par exemple, *secret*).

**Figure 2 - Création d’un nouvel utilisateur**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Vous créez de nouveaux rôles en habilitantant d’abord les rôles et en définissant un ou plusieurs rôles. Activez les rôles en cliquant sur le lien **des rôles Enable.** Ensuite, créez un rôle nommé *Administrateurs* en cliquant sur le lien **Créer ou gérer les rôles** (voir la figure 3).

**Figure 3 - Création d’un nouveau rôle**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Enfin, créez un nouvel utilisateur nommé Sally et associez Sally au rôle des administrateurs en cliquant sur le lien Créer l’utilisateur et en sélectionnant les administrateurs lors de la création de Sally (voir la figure 4).

**Figure 4 - Ajout d’un utilisateur à un rôle**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Quand tout est dit et fait, vous devriez avoir deux nouveaux utilisateurs nommés Stephen et Sally. Vous devez également avoir un nouveau rôle nommé Administrateurs. Sally est membre du rôle des administrateurs et Stephen ne l’est pas.

#### <a name="requiring-authorization"></a>Exiger l’autorisation

Vous pouvez exiger qu’un utilisateur soit authentifié avant que l’utilisateur invoque une action de contrôleur en ajoutant l’attribut [Autoriser] à l’action. Vous pouvez appliquer l’attribut [Autoriser] à une action individuelle de contrôleur ou vous pouvez appliquer cet attribut à toute une classe de contrôleur.

Par exemple, le contrôleur dans la liste 1 expose une action nommée CompanySecrets(). Parce que cette action est décorée de l’attribut [Autoriser], cette action ne peut être invoquée à moins qu’un utilisateur ne soit authentifié.

**Liste 1 - Controllers-HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Si vous invoquez l’action CompanySecrets() en entrant l’URL /Home/CompanySecrets dans la barre d’adresse de votre navigateur, et que vous n’êtes pas un utilisateur authentifié, vous serez redirigé automatiquement vers la vue Login (voir la figure 5).

**Figure 5 - La vue de connexion**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Vous pouvez utiliser la vue Login pour entrer votre nom d’utilisateur et mot de passe. Si vous n’êtes pas un utilisateur enregistré, vous pouvez cliquer sur le lien **de registre** pour naviguer vers la vue du Registre (voir la figure 6). Vous pouvez utiliser la vue Du Registre pour créer un nouveau compte utilisateur.

**Figure 6 - Vue du registre**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Après avoir réussi à vous connecter, vous pouvez voir la vue CompanySecrets (voir la figure 7). Par défaut, vous continuerez à être connecté jusqu’à ce que vous fermiez la fenêtre de votre navigateur.

**Figure 7 - Vue de la sociétéSecrets**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autoriser par nom d’utilisateur ou rôle d’utilisateur

Vous pouvez utiliser l’attribut [Autoriser] pour limiter l’accès à une action de contrôleur à un ensemble particulier d’utilisateurs ou à un ensemble particulier de rôles d’utilisateur. Par exemple, le contrôleur d’accueil modifié dans la liste 2 contient deux nouvelles actions nommées StephenSecrets() et AdministratorSecrets().

**Liste 2 - Controllers-HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Seul un utilisateur avec le nom d’utilisateur Stephen peut invoquer l’action StephenSecrets(). Tous les autres utilisateurs sont redirigés vers la vue Login. La propriété Users accepte une liste de noms de compte d’utilisateur séparée par virgule.

Seuls les utilisateurs dans le rôle des administrateurs peuvent invoquer l’action AdministratorSecrets() . Par exemple, parce que Sally est membre du groupe des administrateurs, elle peut invoquer l’action AdministratorSecrets(). Tous les autres utilisateurs sont redirigés vers la vue Login. La propriété Roles accepte une liste de noms de rôle séparées par virgule.

#### <a name="configuring-authentication"></a>Configurer l’authentification

À ce stade, vous vous demandez peut-être où le compte utilisateur et les informations de rôle sont stockés. Par défaut, les informations sont stockées dans une base de données SQL Express (RANU)\_nommée ASPNETDB.mdf située dans le dossier App Data de votre application MVC. Cette base de données est générée par le cadre ASP.NET automatiquement lorsque vous commencez à utiliser l’adhésion.

Pour voir la base de données ASPNETDB.mdf dans la fenêtre Solution Explorer, vous devez d’abord sélectionner l’option menu Project, Afficher tous les fichiers.

L’utilisation de la base de données SQL Express par défaut est très bien lors du développement d’une application. Très probablement, cependant, vous ne voudrez pas utiliser la base de données ASPNETDB.mdf par défaut pour une application de production. Dans ce cas, vous pouvez modifier l’endroit où les informations du compte utilisateur sont stockées en remplissant les deux étapes suivantes :

1. Ajoutez les objets de base de données de Services d’applications à votre base de données de production - Modifiez votre chaîne de connexion d’application pour indiquer votre base de données de production

La première étape consiste à ajouter tous les objets de base de données nécessaires (tableaux et procédures stockées) à votre base de données de production. La façon la plus simple d’ajouter ces objets à une nouvelle base de données est de profiter de la ASP.NET SQL Server Setup Wizard (voir la figure 8). Vous pouvez lancer cet outil en ouvrant le Visual Studio 2008 Command Prompt à partir du groupe de programme Microsoft Visual Studio 2008 et en exécutant la commande suivante à partir de l’invite de commande :

aspnet\_regsql

**Figure 8 - Le ASP.NET SQL Server Setup Wizard**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

Le ASP.NET SQL Server Setup Wizard vous permet de sélectionner une base de données SQL Server sur votre réseau et d’installer tous les objets de base de données requis par les services d’application ASP.NET. Le serveur de base de données n’est pas tenu d’être situé sur votre machine locale.

> [!NOTE]
> Si vous ne souhaitez pas utiliser le ASP.NET SQL Server Setup Wizard, vous pouvez trouver des scripts SQL pour ajouter les objets de base de données des services d’application dans le dossier suivant :
> 
> 
> C : Windows-Microsoft.NET-Framework-v2.0.50727

Après avoir créé les objets de base de données nécessaires, vous devez modifier la connexion de base de données utilisée par votre application MVC. Modifiez la chaîne de connexion ApplicationServices dans votre fichier de configuration web (web.config) afin qu’il indique la base de données de production. Par exemple, la connexion modifiée dans La liste 3 points à une base de données nommée MyProductionDB (la chaîne de connexion d’origine ApplicationServices a été commentée).

**Liste 3 - Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configurer les autorisations de base de données

Si vous utilisez la sécurité intégrée pour vous connecter à votre base de données, vous devrez ajouter le compte utilisateur Windows correct en tant que connexion à votre base de données. Le compte correct dépend si vous utilisez le serveur de développement ASP.NET ou les services d’information Internet comme serveur web. Le compte utilisateur correct dépend également de votre système d’exploitation.

Si vous utilisez le serveur de développement ASP.NET (le serveur Web par défaut utilisé par Visual Studio), votre application s’exécute dans le contexte de votre compte utilisateur Windows. Dans ce cas, vous devez ajouter votre compte utilisateur Windows comme identifiant serveur de base de données.

Alternativement, si vous utilisez les services d’information Internet, vous devez ajouter soit le compte ASPNET ou le compte NT AUTHORITY/NETWORK SERVICE en tant que connexion serveur de base de données. Si vous utilisez Windows XP, ajoutez le compte ASPNET comme connexion à votre base de données. Si vous utilisez un système d’exploitation plus récent, tel que Windows Vista ou Windows Server 2008, ajoutez le compte NT AUTHORITY/NETWORK SERVICE comme connexion de base de données.

Vous pouvez ajouter un nouveau compte utilisateur à votre base de données en utilisant Microsoft SQL Server Management Studio (voir figure 9).

**Figure 9 - Création d’une nouvelle connexion Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Après avoir créé la connexion requise, vous devez cartographier la connexion à un utilisateur de base de données avec les bons rôles de base de données. Double-cliquez sur la connexion et sélectionnez l’onglet Map d’utilisateur. Sélectionnez un ou plusieurs rôles de base de données de services d’application. Par exemple, afin d’authentifier les utilisateurs,\_\_vous devez activer le rôle de base de base d’adhésion aspnet BasicAccess. Afin de créer de nouveaux utilisateurs, vous\_devez\_activer le rôle de base de données Aspnet Membership FullAccess (voir la figure 10).

**Figure 10 - Ajout de rôles de base de données sur les services d’applications**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Récapitulatif

Dans ce tutoriel, vous avez appris à utiliser l’authentification des formes lors de la construction d’une application MVC ASP.NET. Tout d’abord, vous avez appris à créer de nouveaux utilisateurs et de nouveaux rôles en profitant de l’outil d’administration du site Web. Ensuite, vous avez appris à utiliser l’attribut [Autoriser] pour empêcher les utilisateurs non autorisés d’invoquer des actions de contrôleur. Enfin, vous avez appris à configurer votre application MVC pour stocker des informations utilisateur et rôle dans une base de données de production.

> [!div class="step-by-step"]
> [Suivant précédent](preventing-javascript-injection-attacks-cs.md)
> [Next](authenticating-users-with-windows-authentication-vb.md)
