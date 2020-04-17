---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Adhésion ( fr) Microsoft Docs
author: rick-anderson
description: ASP.NET l’adhésion s’appuie sur le succès du modèle d’authentification des formulaires à partir de ASP.NET 1.x. ASP.NET l’authentification des formes offre un moyen pratique d’incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: ed48c11cbd483de088239bad7c2452b7fc60a1cf
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543767"
---
# <a name="membership"></a>Membership

par [Microsoft](https://github.com/microsoft)

> ASP.NET l’adhésion s’appuie sur le succès du modèle d’authentification des formulaires à partir de ASP.NET 1.x. ASP.NET l’authentification des formulaires fournit un moyen pratique d’intégrer un formulaire de connexion dans votre application ASP.NET et de valider les utilisateurs par rapport à une base de données ou à un autre magasin de données.

ASP.NET l’adhésion s’appuie sur le succès du modèle d’authentification des formulaires à partir de ASP.NET 1.x. ASP.NET l’authentification des formulaires fournit un moyen pratique d’intégrer un formulaire de connexion dans votre application ASP.NET et de valider les utilisateurs par rapport à une base de données ou à un autre magasin de données. Les membres de la classe FormsAuthentication permettent de manipuler des cookies pour l’authentification, de vérifier une connexion valide, de connecter un utilisateur, etc. Toutefois, la mise en œuvre de l’authentification des formulaires dans une application ASP.NET 1.x peut nécessiter une bonne quantité de code.

L’adhésion à ASP.NET 2.0 est un progrès majeur par rapport à l’utilisation de l’authentification des formes seule. (L’adhésion est plus robuste lorsqu’elle est associée à l’authentification des formulaires, mais l’utilisation de l’authentification des formulaires n’est pas une exigence.) Comme vous le verrez bientôt, vous pouvez utiliser ASP.NET l’adhésion et les contrôles de connexion dans ASP.NET 2.0 pour implémenter un système d’adhésion puissant sans écrire beaucoup de code du tout.

## <a name="implementing-membership-in-aspnet-20"></a>Mise en œuvre de l’adhésion à ASP.NET 2.0

L’adhésion est mise en œuvre en suivant quatre étapes. Gardez à l’esprit qu’il existe de nombreuses sous-étapes qui sont impliqués ainsi que la configuration facultative qui peut être mis en œuvre ainsi. Ces étapes visent à illustrer l’ensemble de la configuration de l’adhésion.

1. Créez votre base de données d’adhésion (si SQL Server est utilisé comme magasin d’adhésion.)
2. Spécifiez les options d’adhésion dans vos fichiers de configuration d’applications. (L’adhésion est activée par défaut.)
3. Déterminez le type de magasin d’adhésion que vous souhaitez utiliser. Les options sont : 

    - Serveur SQL Microsoft (version 7.0 ou plus tard)
    - Magasin d’annuaire actif
    - Fournisseur d’adhésion personnalisé
4. Configurer l’application pour ASP.NET l’authentification des formulaires. Une fois de plus, l’adhésion est conçue pour tirer parti de l’authentification des formulaires, mais l’utilisation de l’authentification des formes n’est pas une exigence.
5. Définissez les comptes d’utilisateurs pour les rôles d’adhésion et de configuration si désiré.

## <a name="creating-the-membership-database"></a>Création de la base de données d’adhésion

Si vous utilisez SQL Server 7.0 ou plus tard comme votre\_magasin d’adhésion, vous pouvez utiliser l’utilitaire aspnet regsql (disponible le plus facilement à partir du Visual Studio .NET 2005 Command Prompt) pour configurer votre base de données. L’utilitaire\_aspnet regsql peut être utilisé comme un outil d’invite de commande ou via un assistant GUI. La méthode assistante est le moyen le plus simple de configurer votre base de données. Pour accéder à l’assistant, il suffit d’exécuter la commande suivante:

`aspnet_regsql W`

Une fois que vous exécutez cette commande, vous serez présenté avec le ASP.NET SQL Server Setup Wizard comme indiqué ci-dessous.

![](membership/_static/image1.jpg)

**La figure 1**

Le ASP.NET SQL Server Setup Wizard crée le site Web dans l’exemple que vous spécifiez dans l’assistant. Toutefois, ASP.NET utilisera la chaîne de connexion dans le fichier machine.config pour se connecter à votre base de données. Par défaut, cette chaîne de connexion indiquera une instance SQL Server 2005, donc si vous utilisez une instance SQL Server 2000 ou SQL Server 7.0, vous devrez modifier la chaîne de connexion dans le fichier machine.config. Cette chaîne de connexion peut être située ici:

[!code-xml[Main](membership/samples/sample1.xml)]

Malheureusement, si vous ne modifiez pas la chaîne de connexion, ASP.NET ne vous donnera pas une erreur descriptive. Il va juste continuer à se plaindre en disant que vous n’avez pas créé la base de données. Dans le cas ci-dessus, j’ai modifié la chaîne de connexion pour pointer vers mon local SQL Server 2000 instance.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Spécifier la configuration et l’ajout d’utilisateurs et de rôles

La prochaine étape dans la configuration de l’adhésion est d’ajouter les informations nécessaires au fichier web.config de l’application. Dans ASP.NET 1.x, la modification du fichier web.config a parfois été difficile en raison de l’utilisation de lowerCamelCase et le manque d’Intellisense. Visual Studio .NET 2005 rend la tâche beaucoup plus facile avec Intellisense pour les fichiers de configuration, mais ASP.NET 2.0 va plus loin en fournissant une interface Web pour l’édition de fichiers de configuration.

Vous pouvez lancer l’interface Web en cliquant sur le bouton ASP.NET Configuration sur la barre d’outils Solution Explorer comme indiqué ci-dessous. Vous pouvez également lancer l’interface Web via des pop-ups qui s’affichent lorsque les commandes Login sont insérées.

![](membership/_static/image2.jpg)

**Figure 2**

Cela lance l’outil ASP.NET d’administration de sites Web ci-dessous. La ASP.NET’administration du site Web est une interface à quatre onglets qui facilite la gestion des paramètres d’application. Les onglets suivants sont disponibles :

- **Page d'accueil**
- **Sécurité** Configurer les utilisateurs, les rôles et l’accès.
- **Demande** Configurer les paramètres d’application.
- **Fournisseur** Configurez et testez votre fournisseur d’adhésion aux applications.

L’outil d’administration de sites Web vous permet de créer facilement de nouveaux utilisateurs, de créer de nouveaux rôles et de gérer les utilisateurs et les rôles. Cette capacité n’est pas disponible dans l’interface Windows. L’interface Windows vous permet de définir facilement les paramètres d’autorisation et d’ajouter, supprimer et gérer les fournisseurs, des fonctionnalités qui ne figurent pas dans l’outil d’administration du site Web.

Pour lancer l’interface Windows, ouvrez le snap-in des services d’information Internet, cliquez à droite sur votre application et choisissez des propriétés. Cliquez sur l’onglet ASP.NET, puis cliquez sur le bouton Configuration d’édition. (L’application doit être en cours d’exécution sous ASP.NET 2.0 pour que le bouton Configuration d’édition soit activé. Vous pouvez configurer la version ASP.NET dans le dialogue ASP.NET ainsi.) Le dialogue ASP.NET Configuration Paramètres s’affiche comme indiqué ci-dessous.

![](membership/_static/image3.jpg)

**Figure 3**

Sur l’onglet Général, les chaînes de connexion et les paramètres d’application sont répertoriés. Tous les paramètres en italique sont définis dans un fichier de configuration parent (soit le site machine.config ou un web.config à un niveau supérieur) et les paramètres non en italiques proviennent du fichier de configuration des applications. Si un paramètre est ajouté, supprimé ou modifié au niveau de l’application, ASP.NET ajoutera, supprimera ou modifiera le paramètre aux niveaux d’application web.config au lieu de supprimer le paramètre du fichier de configuration dont il est hérité.

L’onglet Authentification est indiqué ci-dessous. C’est là que vous configurer vos paramètres d’adhésion. Les paramètres d’authentification des formulaires, les fournisseurs d’adhésion et les fournisseurs de rôles peuvent être configurés ici.

![](membership/_static/image4.jpg)

**Figure 4**

## <a name="implementing-membership-in-your-application"></a>Mise en œuvre de l’adhésion à votre demande

La façon la plus simple d’implémenter ASP.NET adhésion 2.0 à votre application est d’utiliser les contrôles Logon fournis. Cette méthode vous permet d’implémenter les bases de ASP.NET’adhésion 2.0 sans écrire de code du tout.

Les commandes Logon suivantes sont disponibles en ASP.NET 2.0 :

## <a name="login-control"></a>Contrôle de la connexion

Le contrôle Login fournit une interface pour que quelqu’un se connecte à votre système d’adhésion. Il vous fournit un nom d’utilisateur et une boîte texte de mot de passe et un bouton de connexion. Beaucoup d’autres fonctionnalités courantes telles qu’un lien pour s’inscrire pour les personnes qui ne l’ont pas encore fait, une case à cocher qui permet à l’utilisateur de se connecter automatiquement lors de visites ultérieures, un lien pour un rappel de mot de passe, etc. Toutes les fonctionnalités du contrôle Login sont personnalisables via les propriétés du contrôle.

Dans ASP.NET 1.x, les développeurs ont dû écrire une bonne quantité de code pour faire un examen lors de l’utilisation de l’authentification des formes. Avec ASP.NET’adhésion 2.0, vous pouvez valider les utilisateurs sans écrire de code du tout. ASP.NET fera automatiquement la recherche de l’utilisateur pour vous. (Si vous utilisez le contrôle Login sans utiliser ASP.NET’adhésion, vous pouvez utiliser la méthode **OnAuthenticate** pour valider l’utilisateur.)

## <a name="loginview-control"></a>Contrôle LoginView

Le contrôle LoginView est un contrôle modélé qui fournit deux modèles par défaut; AnonymousTemplate et le LoggedInTemplate. Le modèle affiché est déterminé par la question de savoir si l’utilisateur est connecté ou non à votre système d’adhésion. Ce contrôle est généralement utilisé pour afficher un contrôle De connexion lorsqu’un utilisateur ne s’est pas encore connecté et qu’un contrôle LoginStatus et/ou d’autres contrôles de connexion lorsque l’utilisateur s’est connecté. Si vous utilisez la gestion des rôles dans votre application ASP.NET, le contrôle LoginView peut afficher un modèle spécifique basé sur le rôle des utilisateurs. (Plus sur ASP.NET gestion des rôles sera couverte plus tard.)

## <a name="passwordrecovery-control"></a>Contrôle de PasswordRecovery

Le contrôle PasswordRecovery permet aux utilisateurs de recevoir un e-mail avec son mot de passe actuel ou de réinitialiser son mot de passe. Le texte clair et les mots de passe cryptés peuvent être récupérés et envoyés par courriel aux utilisateurs. Si le mot de passe est haché, il ne peut pas être récupéré. Au lieu de cela, l’utilisateur sera tenu d’effectuer une réinitialisation de mot de passe.

## <a name="loginstatus-control"></a>LoginStatus Control

Le contrôle LoginStatus est utilisé pour afficher un indicateur de connexion aux utilisateurs qui ne sont pas connectés et un indicateur de logout aux utilisateurs qui sont actuellement connectés. La propriété Request.IsAuthenticated est utilisée pour déterminer quel indicateur afficher. L’indicateur affiché par le contrôle LoginStatus peut être le texte (implémenté via les propriétés **LoginText** et **LogoutText)** ou des images (implémentées via les propriétés **LoginImageUrl** et **LogoutImageUrl).)**

Lorsqu’un utilisateur se connecte via le contrôle LoginStatus, il est redirigé vers l’URL spécifiée par la propriété **LogoutPageUrl.** Si cette propriété n’est pas définie, la page actuelle est actualisée. Étant donné que le site est probablement protégé par l’authentification des formulaires, la mise à jour de la page actuelle redirigera l’utilisateur vers la page de connexion du site.

## <a name="loginname-control"></a>Contrôle de LoginName

Le contrôle LoginName affiche le nom d’utilisateur de l’utilisateur actuellement connecté au site.

## <a name="createuserwizard-control"></a>CreateUserWizard Control

Le contrôle CreateUserWizard offre aux utilisateurs un moyen pratique de s’inscrire à votre système d’adhésion. Vous pouvez ajouter des étapes (implémentées comme une collection de WizardSteps) via l’interface affichée ci-dessous.

![](membership/_static/image5.jpg)

**Figure 5**

Le CreateUserWizard est un contrôle modélique qui dérive de la classe Wizard et fournit les modèles suivants:

- **HeaderTemplate (en)** Ce modèle contrôle l’apparence de l’en-tête de l’assistant.
- **SidebarTemplate** Ce modèle contrôle l’apparence de la barre latérale de l’assistant.
- **StartNavigationTemplate StartNavigationTemplate** Ce modèle contrôle l’apparence de la navigation sont de l’assistant à l’étape de départ.
- **StepNavigationTemplate** Ce modèle contrôle l’apparence de la zone de navigation lorsqu’il n’est pas dans l’étape de départ ou de finition.
- **FiniNavigationTemplate** Ce modèle contrôle l’apparence de la zone de navigation lorsque vous êtes sur l’étape d’arrivée.

En outre, pour chaque étape que vous ajoutez à l’Assistant, ASP.NET créera un modèle personnalisé qui contient à la fois un ContentTemplate et un CustomNavigationTemplate pour cette étape. Pour plus de détails sur la personnalisation de la CreateUserWizard, consultez la documentation VS.NET 2005 :

## <a name="changepassword-control"></a>Contrôle ChangePassword

Le contrôle ChangePassword permet aux utilisateurs de changer son mot de passe. Si la propriété DisplayUserName est vraie (elle est fausse par défaut), l’utilisateur peut changer son mot de passe lorsqu’il n’est pas connecté. Si l’utilisateur *est* déjà connecté et que la propriété DisplayUserName est vraie, l’utilisateur sera en mesure de modifier le mot de passe d’un autre utilisateur qui n’est pas connecté à condition qu’il connaisse l’identifiant utilisateur de cet utilisateur.

Gardez à l’esprit que si vous voulez que les utilisateurs soient en mesure de changer de mot de passe sans avoir à vous connecter, vous devrez vous assurer que la page sur laquelle le contrôle ChangePassword est affiché permet un accès anonyme. Évidemment, les utilisateurs devront fournir leur ancien mot de passe afin de changer leur mot de passe.

## <a name="role-management"></a>Gestion des rôles

La gestion des rôles vous permet d’attribuer des utilisateurs à un rôle particulier, puis de restreindre l’accès à certains fichiers ou dossiers en fonction de ce rôle. La gestion des rôles fournit également une API afin que vous puissiez déterminer de façon programmatique le rôle de quelqu’un ou déterminer tous les utilisateurs dans un rôle particulier et répondre en conséquence.

La gestion des rôles n’est pas une exigence dans ASP.NET membres, et l’adhésion n’est pas une exigence pour utiliser la gestion des rôles. Cependant, les deux se complètent bien et il est probable que les développeurs les utiliseront en conjonction les uns avec les autres.

Pour activer la gestion des rôles dans votre application, effectuez la modification suivante dans votre fichier web.config :

[!code-xml[Main](membership/samples/sample2.xml)]

Lorsque **l’attribut cacheRolesInCookie** est configuré pour vrai, ASP.NET cache une adhésion au rôle des utilisateurs dans un cookie sur le client. Cela permet aux examens de rôle de se produire sans appels dans le RoleProvider. Lors de l’utilisation de cet attribut, les développeurs sont encouragés à s’assurer que **l’attribut cookieProtection** est réglé à tous. (C’est le paramètre par défaut.) Cela garantit que les données de cookies sont cryptées et permet de s’assurer que le contenu des cookies n’a pas été modifié. Les rôles peuvent être ajoutés à l’aide de l’outil d’administration du site Web. Il vous permet de définir facilement les rôles, de configurer l’accès à certaines parties du site en fonction de ces rôles et d’attribuer aux utilisateurs des rôles.

![](membership/_static/image6.jpg)

**Figure 6**

Comme indiqué ci-dessus, de nouveaux rôles peuvent être ajoutés en entrant simplement le nom du rôle, puis en cliquant sur Add Role. Les rôles existants peuvent être gérés ou supprimés en cliquant sur le lien approprié dans la liste des rôles existants.

Lorsque vous gérez un rôle, vous pouvez ajouter ou supprimer les utilisateurs tels qu’ils sont indiqués ci-dessous.

![](membership/_static/image7.jpg)

**Figure 7**

En vérifiant que la case à cocher de l’utilisateur est dans la boîte de contrôle, vous pouvez facilement ajouter un utilisateur à un rôle spécifique. ASP.NET mettra à jour automatiquement votre base de données d’adhésion avec les entrées appropriées. Vous voudrez également configurer les règles d’accès pour votre application. ASP.NET développeurs 1.x sont familiers avec le &lt;&gt; faire via l’élément d’autorisation dans le fichier web.config, et cette option est toujours disponible en ASP.NET 2.0. Cependant, il est plus facile de gérer les règles d’accès à l’aide de l’outil d’administration de site Web comme indiqué ci-dessous.

![](membership/_static/image8.jpg)

**Figure 8**

Dans ce cas, le dossier de l’Administration est mis en évidence (son difficile à voir parce que l’outil le met en évidence en gris clair) et le rôle des administrateurs a été accordé l’accès. Tous les autres utilisateurs sont refusés. Vous pouvez cliquer sur l’icône de tête pour sélectionner une règle, puis utiliser les boutons Move Up et Move Down pour organiser les règles. Comme pour l’élément &lt;&gt; d’autorisation ASP.NET, les règles sont traitées dans l’ordre dans lequel elles apparaissent. En d’autres termes, si l’ordre des règles dans le coup ci-dessus ont été inversés, personne n’aurait accès au dossier de l’Administration parce que la première règle que ASP.NET rencontrerait serait la règle qui refuse tout le monde au dossier.

ASP.NET 2.0 ajoute un fichier web.config au dossier pour lequel vous spécifiez une règle d’accès. Les règles d’accès peuvent être modifiées via le fichier de configuration ou via l’outil d’administration du site Web. En d’autres termes, l’outil d’administration du site Web est simplement une interface par laquelle le fichier de configuration peut être édité dans un environnement convivial.

## <a name="using-roles-in-code"></a>Utilisation des rôles dans le code

L’API pour la gestion des rôles n’a pas changé depuis la version 1.x. La méthode **IsInRole** est utilisée pour déterminer si un utilisateur joue un rôle particulier.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET crée également un rôle principal en tant que membre du contexte actuel. L’objet RolePrincipal peut être utilisé pour obtenir tous les rôles auxquels l’utilisateur appartient comme suit :

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Utilisation de RoleGroups avec le contrôle LoginView

Maintenant que vous avez une compréhension de la gestion des rôles et de l’adhésion, permet de discuter brièvement de la façon dont le contrôle LoginView tire parti de cette capacité dans ASP.NET 2.0. Comme nous l’avons déjà mentionné, le contrôle LoginView est un contrôle modélisant qui contient deux modèles par défaut; AnonymousTemplate et le LoggedInTemplate. Dans le dialogue LoginView Tasks est un lien (indiqué ci-dessous) qui vous permet de modifier RoleGroups.

![](membership/_static/image9.jpg)

**Figure 9**

Chaque objet RoleGroup contient un tableau de chaînes qui définit les rôles auxquels RoleGroup s’applique. Pour ajouter un nouveau Groupe de rôle au contrôle LoginView, cliquez sur le lien Edit RoleGroups. Dans l’image ci-dessus, vous pouvez voir que j’ai ajouté un nouveau Groupe de rôle pour les administrateurs. En sélectionnant ce RoleGroup (RoleGroup[0]) à partir de la décroche vues, je peux configurer un modèle qui ne sera affiché qu’aux membres du rôle des administrateurs. Dans l’image ci-dessous, j’ai ajouté un nouveau RoleGroup qui s’applique aux membres du rôle de vente et le rôle de distribution. Cela ajoute un deuxième Groupe de rôle à la baisse vues dans le dialogue LoginView Tasks et tout ce qui est ajouté à ce modèle sera visible par tout utilisateur dans le rôle de vente ou de distribution.

![](membership/_static/image10.jpg)

**Figure 10**

## <a name="overriding-the-existing-membership-provider"></a>L’adhésion au fournisseur existant

Il ya quelques façons que vous pouvez étendre la fonctionnalité de ASP.NET membres. Tout d’abord, vous pouvez évidemment modifier la fonctionnalité existante de la classe SqlMembershipProvider en héritant de lui et en dominant ses méthodes. Par exemple, si vous souhaitez implémenter vos propres fonctionnalités lorsque des utilisateurs sont créés, vous pouvez créer votre propre classe qui hérite de SqlMembershipProvider comme suit :

[!code-csharp[Main](membership/samples/sample5.cs)]

Si, par contre, vous souhaitez créer votre propre fournisseur (pour stocker vos informations d’adhésion dans une base de données Access, par exemple), vous pouvez créer votre propre fournisseur.

## <a name="creating-your-own-membership-provider"></a>Créer votre propre fournisseur d’adhésion

Pour créer votre propre fournisseur d’adhésion, vous devrez d’abord créer une classe qui hérite de la classe MembershipProvider. Si vous utilisez VB.NET, Visual Studio 2005 ajoutera les talons pour toutes les méthodes dont vous avez besoin pour passer outre. Si vous utilisez C, c’est à vous d’ajouter les talons.

Vous devrez passer outre ce qui suit :

- Propriété ApplicationName
- Fonction ChangePassword
- ChangePasswordQuestionAndAnswer fonction
- Créer la fonction De l’us
- Fonction DeleteUser
- Propriété EnablePasswordReset
- Propriété EnablePasswordRetrieval
- FindUsersByemail fonction
- FindUsersByName fonction
- Fonction GetAllUsers
- Fonction GetNumberOfUsersOnline
- Fonction GetPassword
- Fonction GetUser
- Fonction GetUserNameByEmail
- Propriété MaxInvalidPasswordAttempts
- MinRequiredNonAlphanumericCharacters propriété
- MinRequiredPasswordLength propriété
- Propriété PasswordAttemptWindow
- Propriété PasswordFormat
- PasswordStrengthRegularExpression propriété
- ExigeQuestionAndAnswer propriété
- Nécessite une propriété UniqueEmail
- Fonction ResetPassword
- Débloquer la fonction utilisateur
- Fonction UpdateUser
- Validation De la fonction De l’us

C’est tout à fait une liste à mettre en œuvre en tant que développeur C. Vous pouvez trouver plus facile de créer la classe en VB.NET sans aucune implémentation, puis utiliser .NET Reflector ou un outil similaire pour convertir le code en C.

La chaîne de connexion et d’autres propriétés doivent être réglées à leurs défauts dans la méthode Initialize. (La méthode Initialize est congédiée lorsque le fournisseur est chargé au moment de l’exécution.) Le deuxième paramètre de la méthode Initialize est de type System.Collections.Specialized.NameValueCollection et est une référence à l’élément &lt;d’ajout&gt; qui est associé à votre fournisseur personnalisé dans le fichier web.config. Cette entrée ressemble à ce qui suit:

[!code-xml[Main](membership/samples/sample6.xml)]

Voici un exemple de la méthode Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Afin de valider l’utilisateur lorsqu’il soumet votre formulaire de connexion, vous devrez utiliser la méthode ValidateUser. Cette méthode s’allume lorsque l’utilisateur clique sur le bouton de connexion dans le contrôle De la connexion. Vous placez votre code qui ne l’utilisateur recherche dans cette méthode.

Comme vous pouvez le voir, l’écriture de votre propre fournisseur d’adhésion n’est pas difficile et vous permet d’étendre cette fonctionnalité puissante de ASP.NET 2.0.
