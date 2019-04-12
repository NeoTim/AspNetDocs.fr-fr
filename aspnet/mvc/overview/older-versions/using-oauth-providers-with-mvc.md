---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: À l’aide de fournisseurs OAuth avec MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application web ASP.NET MVC 4 qui permet aux utilisateurs de se connecter avec leurs informations d’identification à partir d’un fournisseur externe, tel que Facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: c2fe74c3d7b1aa0d230f1893f6ba7dcaa7a88419
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59396979"
---
# <a name="using-oauth-providers-with-mvc-4"></a>Utilisation de fournisseurs OAuth avec MVC 4

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment créer une application web ASP.NET MVC 4 qui permet aux utilisateurs de se connecter avec des informations d’identification à partir d’un fournisseur externe, tels que Facebook, Twitter, Microsoft ou Google et l’intégrer ensuite certaines des fonctionnalités à partir de ces fournisseurs dans votre application Web. Par souci de simplicité, ce didacticiel se concentre sur l’utilisation des informations d’identification à partir de Facebook.
> 
> Pour utiliser les informations d’identification externes dans une application web ASP.NET MVC 5, consultez [créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et OpenID Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> L’activation de ces informations d’identification dans vos sites web de fournit un avantage significatif, car des millions d’utilisateurs disposent déjà de comptes avec ces fournisseurs externes. Ces utilisateurs peuvent être plus de chances de s’inscrire à votre site si elles n’ont pas à créer et à mémoriser un nouvel ensemble d’informations d’identification. En outre, une fois un utilisateur s’est connecté via un de ces fournisseurs, vous pouvez incorporer des opérations réseau sociales à partir du fournisseur.


## <a name="what-youll-build"></a>Vous allez générer

Il existe deux principaux objectifs dans ce didacticiel :

1. Permettre aux utilisateurs de se connecter avec des informations d’identification d’un fournisseur OAuth.
2. Récupérer des informations de compte à partir du fournisseur et intégrer ces informations à l’inscription de compte pour votre site.

Bien que les exemples de ce didacticiel vous concentrer sur l’utilisation de Facebook comme fournisseur d’authentification, vous pouvez modifier le code pour utiliser un des fournisseurs. Les étapes pour implémenter n’importe quel fournisseur sont très similaires à celles que vous voyez dans ce didacticiel. Vous remarquez des différences significatives lors des appels directs à l’API du fournisseur défini.

## <a name="prerequisites"></a>Prérequis

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) ou [Microsoft Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Ou

- Microsoft Visual Studio 2010 SP1 ou [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

En outre, cette rubrique part du principe que vous avez des connaissances de base sur ASP.NET MVC et Visual Studio. Si vous avez besoin d’une introduction à ASP.NET MVC 4, consultez [Introduction à ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Créer le projet

Dans Visual Studio, créez une nouvelle Application Web ASP.NET MVC 4 et nommez-le &quot;OAuthMVC&quot;. Vous pouvez cibler .NET Framework 4.5 ou 4.

![créer le projet](using-oauth-providers-with-mvc/_static/image1.png)

Dans la fenêtre Nouveau projet ASP.NET MVC 4, sélectionnez **Application Internet** et laissez **Razor** en tant que le moteur d’affichage.

![Sélectionnez l’Application Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Activer un fournisseur

Lorsque vous créez une application web MVC 4 avec le modèle d’Application Internet, le projet est créé avec un fichier nommé AuthConfig.cs dans l’application\_dossier de démarrage.

![Fichier de AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Le fichier AuthConfig contient le code pour inscrire les clients pour les fournisseurs d’authentification externes. Par défaut, ce code est commenté pour aucun des fournisseurs externes sont activés.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Vous devez supprimez les commentaires de ce code pour utiliser le client d’authentification externe. Vous supprimez les commentaires uniquement les fournisseurs que vous souhaitez inclure dans votre site. Pour ce didacticiel, vous allez uniquement activer les informations d’identification Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Notez que, dans l’exemple ci-dessus est que la méthode inclut des chaînes vides pour les paramètres d’inscription. Si vous essayez d’exécuter l’application maintenant, l’application lève une exception d’argument, car les chaînes vides ne sont pas autorisés pour les paramètres. Pour fournir des valeurs valides, vous devez inscrire votre site web avec les fournisseurs externes, comme indiqué dans la section suivante.

## <a name="registering-with-an-external-provider"></a>L’inscription auprès d’un fournisseur externe

Pour authentifier les utilisateurs avec les informations d’identification à partir d’un fournisseur externe, vous devez inscrire votre site web avec le fournisseur. Lorsque vous inscrivez votre site, vous recevrez les paramètres (tels que la clé ou id et clé secrète) pour inclure lors de l’inscription du client. Vous devez disposer d’un compte avec les fournisseurs que vous souhaitez utiliser.

Ce didacticiel n’affiche pas toutes les étapes que vous devez effectuer pour vous inscrire avec ces fournisseurs. Les étapes ne sont généralement pas difficile. Pour enregistrer correctement votre site, suivez les instructions fournies sur ces sites. Pour commencer l’inscription de votre site, consultez le site de développeur pour :

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Lors de l’inscription de votre site avec Facebook, vous pouvez fournir &quot;localhost&quot; pour le domaine du site et `&quot;http://localhost/&quot;` pour l’URL, comme indiqué dans l’image ci-dessous. En utilisant localhost fonctionne avec la plupart des fournisseurs, mais ne fonctionne pas actuellement avec le fournisseur de Microsoft. Pour le fournisseur de Microsoft, vous devez inclure une URL de site web valide.

![inscription de site](using-oauth-providers-with-mvc/_static/image4.png)

Dans l’image précédente, les valeurs de l’id d’application, clé secrète d’application et e-mail de contact ont été supprimés. Lorsque vous inscrivez en fait votre site, ces valeurs seront présents. Vous devez noter les valeurs pour l’id d’application et secret d’application car vous allez les ajouter à votre application.

## <a name="creating-test-users"></a>Création d’utilisateurs de test

Si vous occupez pas à l’aide d’un compte Facebook existant pour tester votre site, vous pouvez ignorer cette section.

Vous pouvez facilement créer des utilisateurs de test pour votre application au sein de la page de gestion d’application Facebook. Vous pouvez utiliser ces comptes pour vous connecter à votre site de test. Vous créez des utilisateurs de test en cliquant sur le **rôles** lien dans le volet de navigation gauche et le clic le **créer** lien.

![créer des utilisateurs de test](using-oauth-providers-with-mvc/_static/image5.png)

Le site Facebook crée automatiquement le nombre de comptes de test que vous demandez.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Ajout d’id d’application et la clé secrète à partir du fournisseur

Maintenant que vous avez reçu l’id et la clé secrète à partir de Facebook, revenez au fichier AuthConfig et ajoutez-les en tant que les valeurs de paramètre. Les valeurs indiquées ci-dessous ne sont pas des valeurs réelles.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Se connecter avec des informations d’identification externes

Voilà, il vous suffit pour activer les informations d’identification externes dans votre site. Exécutez votre application et cliquez sur le lien de connexion dans le coin supérieur droit. Le modèle reconnaît automatiquement que vous avez inscrit Facebook en tant que fournisseur et inclut un bouton pour le fournisseur. Si vous inscrivez plusieurs fournisseurs, un bouton pour chacun d'entre eux est automatiquement inclus.

![connexion externe](using-oauth-providers-with-mvc/_static/image6.png)

Ce didacticiel ne couvre pas comment personnaliser le journal des boutons pour les fournisseurs externes. Pour plus d’informations, consultez [personnalisation de l’interface utilisateur de connexion lors de l’utilisation de OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Cliquez sur le bouton de Facebook pour vous connecter avec leurs identifiants Facebook. Lorsque vous sélectionnez un des fournisseurs externes, vous êtes redirigé vers ce site et vous y êtes invité par ce service pour vous connecter.

L’illustration suivante montre l’écran de connexion pour Facebook. Il remarque que vous utilisez votre compte Facebook pour vous connecter à un site nommé oauthmvcexample.

![authentification Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Une fois connecté avec les informations d’identification Facebook, une page informe l’utilisateur que le site aura accès aux informations de base.

![demander l’autorisation](using-oauth-providers-with-mvc/_static/image8.png)

Après avoir sélectionné **accéder à l’application**, l’utilisateur doit enregistrer pour le site. L’illustration suivante montre la page d’inscription après qu’un utilisateur a ouvert une session avec leurs identifiants Facebook. Le nom d’utilisateur est généralement préremplie avec un nom à partir du fournisseur.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Cliquez sur **inscrire** pour terminer l’inscription. Fermez le navigateur.

Vous pouvez voir que le nouveau compte a été ajouté à votre base de données. Dans l’Explorateur de serveurs, ouvrez le **DefaultConnection** de base de données et ouvrez le **Tables** dossier.

![tables de base de données](using-oauth-providers-with-mvc/_static/image10.png)

Avec le bouton droit le **UserProfile** de table et sélectionnez **afficher les données de Table**.

![afficher les données](using-oauth-providers-with-mvc/_static/image11.png)

Vous verrez le nouveau compte que vous avez ajouté. Examiner les données dans **page Web\_OAuthMembership** table. Vous verrez plus de données liées au fournisseur externe pour le compte que vous venez d’ajouter.

Si vous souhaitez uniquement activer l’authentification externe, vous avez terminé. Toutefois, vous pouvez intégrer à partir du fournisseur d’informations dans le nouveau processus de l’inscription de l’utilisateur, comme indiqué dans les sections suivantes.

## <a name="create-models-for-additional-user-information"></a>Créer des modèles pour des informations utilisateur supplémentaires

Comme vous remarqué dans les sections précédentes, vous n’avez pas besoin récupérer des informations supplémentaires pour l’inscription de compte intégré fonctionner. Toutefois, la plupart de ces fournisseurs passer revenir plus d’informations sur l’utilisateur. Les sections suivantes vous montrent comment conserver ces informations et l’enregistrer dans une base de données. Plus précisément, vous conserverez les valeurs pour le nom complet de l’utilisateur, l’URI de page web personnelle de l’utilisateur et une valeur qui indique si Facebook a vérifié le compte.

Vous allez utiliser [Migrations Code First](https://msdn.microsoft.com/data/jj591621) pour ajouter une table pour stocker des informations utilisateur supplémentaires. Vous ajoutez la table à une base de données existante, donc vous devez d’abord créer un instantané de la base de données actuelle. En créant un instantané de la base de données en cours, vous pourrez créer ultérieurement une migration qui contient uniquement la nouvelle table. Pour créer un instantané de la base de données actuelle :

1. Ouvrez le **Console du Gestionnaire de Package**
2. Exécutez la commande **enable-migrations**
3. Exécutez la commande **ajouter-migration initial – IgnoreChanges**
4. Exécutez la commande **mise à jour la base de données**

À présent, vous allez ajouter les nouvelles propriétés. Dans le dossier Models, ouvrez le fichier AccountModels.cs et trouver la classe RegisterExternalLoginModel. La classe RegisterExternalLoginModel conserve les valeurs renvoyés à partir du fournisseur d’authentification. Ajouter des propriétés nommées FullName et lien, comme indiqué ci-dessous.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Également dans AccountModels.cs, ajoutez une nouvelle classe appelée ExtraUserInformation. Cette classe représente la nouvelle table qui sera créée dans la base de données.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Dans la classe UsersContext, ajoutez le code en surbrillance ci-dessous pour créer une propriété DbSet pour la nouvelle classe.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Vous êtes maintenant prêt à créer la nouvelle table. Ouvrez la Console du Gestionnaire de Package et cette fois :

1. Exécutez la commande **migration ajouter AddExtraUserInformation**
2. Exécutez la commande **mise à jour la base de données**

Il existe maintenant la nouvelle table dans la base de données.

## <a name="retrieve-the-additional-data"></a>Récupérer les données supplémentaires

Il existe deux façons de récupérer des données utilisateur supplémentaires. La première consiste à conserver les données utilisateur qui sont retournées, par défaut, lors de la demande d’authentification. La seconde consiste à appeler l’API du fournisseur en particulier et de demander des informations supplémentaires. Valeurs des FullName et lien sont automatiquement transmises par Facebook. Une valeur qui indique si Facebook a vérifié le compte est extraite via un appel à l’API Facebook. Tout d’abord, vous allez renseigner les valeurs pour FullName et lien, et une version ultérieure, vous obtenez la valeur vérifiée.

Pour récupérer les données utilisateur supplémentaires, ouvrez le **AccountController.cs** de fichiers dans le **contrôleurs** dossier.

Ce fichier contient la logique de journalisation, l’inscription et la gestion des comptes. En particulier, notez les méthodes appelées **ExternalLoginCallback** et **ExternalLoginConfirmation**. Au sein de ces méthodes, vous pouvez ajouter le code pour personnaliser les opérations de connexion externe pour votre application. La première ligne de la **ExternalLoginCallback** méthode contient :

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Autres données utilisateur a été renvoyé dans le **ExtraData** propriété de la **AuthenticationResult** objet qui est retourné à partir de la **verifyauthentication qui se** (méthode). Le client Facebook contienne les valeurs suivantes dans le **ExtraData** propriété :

- ID
- name
- link
- sexe
- accesstoken

Dans la propriété ExtraData les autres fournisseurs ont similaires mais légèrement différentes.

Si l’utilisateur est une nouveauté à votre site, vous récupérer certaines données supplémentaires et transmettre ces données à la vue de confirmation. Le dernier bloc de code dans la méthode est exécuté uniquement si l’utilisateur est une nouveauté de votre site. Remplacez la ligne suivante :

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Avec cette ligne :

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Cette modification inclut simplement les valeurs des propriétés FullName et lien.

Dans le **ExternalLoginConfirmation** (méthode), modifiez le code en surbrillance ci-dessous pour enregistrer les informations utilisateur supplémentaires.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Ajustement de la vue

Les données utilisateur supplémentaires que vous récupérez à partir du fournisseur seront affichera dans la page d’inscription.

Dans le **vues**/**compte** dossier, ouvrez **ExternalLoginConfirmation.cshtml**. Sous le champ existant pour le nom d’utilisateur, ajouter des champs FullName et lien PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Maintenant, vous êtes presque prêt à exécuter l’application et inscrire un nouvel utilisateur avec les informations supplémentaires enregistrées. Vous devez disposer d’un compte qui n’est pas déjà inscrit auprès du site. Vous pouvez utiliser un autre compte de test, ou supprimer les lignes dans le **UserProfile** et **pages Web\_OAuthMembership** tables pour le compte que vous souhaitez réutiliser. En supprimant ces lignes, vous allez vérifier que le compte est inscrit à nouveau.

Exécutez l’application et inscrire le nouvel utilisateur. Notez que cette fois la page de confirmation contient plus de valeurs.

![register](using-oauth-providers-with-mvc/_static/image12.png)

À l’issue de l’inscription, fermez le navigateur. Rechercher dans la base de données à noter les nouvelles valeurs dans le **ExtraUserInformation** table.

## <a name="install-nuget-package-for-facebook-api"></a>Installez le package NuGet pour l’API Facebook

Facebook fournit un [API](https://developers.facebook.com/docs/reference/apis/) que vous pouvez appeler pour effectuer des opérations. Vous pouvez appeler l’API Facebook en dirigeant l’envoi de demandes HTTP, soit à l’aide de l’installation d’un package NuGet qui facilite l’envoi de ces demandes. À l’aide d’un package NuGet est indiqué dans ce didacticiel, mais l’installation de NuGet package n’est pas essentiel. Ce didacticiel explique comment utiliser le package C# SDK Facebook. Il existe des autres packages NuGet permettant l’appel de l’API Facebook.

À partir de la **gérer les Packages NuGet** windows, sélectionnez le package C# SDK Facebook.

![installer le package](using-oauth-providers-with-mvc/_static/image13.png)

Vous allez utiliser le SDK Facebook C# pour appeler une opération qui requiert le jeton d’accès pour l’utilisateur. La section suivante montre comment obtenir le jeton d’accès.

## <a name="retrieve-access-token"></a>Récupérer le jeton d’accès

La plupart de ces fournisseurs transmettre un jeton d’accès après vérification des informations d’identification de l’utilisateur. Ce jeton d’accès est très important, car elle vous permet d’appeler des opérations qui sont disponibles uniquement pour les utilisateurs authentifiés. Par conséquent, récupérer et stocker le jeton d’accès sont essentiel lorsque vous souhaitez fournir davantage de fonctionnalités.

Selon le fournisseur externe, le jeton d’accès peut être valid pour qu’une quantité limitée de temps. Pour vous assurer que vous disposez d’un jeton d’accès valide, vous il récupérera chaque fois que l’utilisateur se connecte et stocker en tant que valeur de session non enregistrer dans une base de données.

Dans le **ExternalLoginCallback** (méthode), le jeton d’accès est également passé dans le **ExtraData** propriété de la **AuthenticationResult** objet. Ajoutez le code en surbrillance à **ExternalLoginCallback** pour enregistrer le jeton d’accès dans le **Session** objet. Ce code est exécuté chaque fois que l’utilisateur se connecte avec un compte Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Bien que cet exemple récupère un jeton d’accès à partir de Facebook, vous pouvez récupérer le jeton d’accès à partir de n’importe quel fournisseur externe via la même clé nommée &quot;accesstoken&quot;.

## <a name="logging-off"></a>Déconnexion

La valeur par défaut **fermeture de session** méthode se connecte l’utilisateur de votre application, mais n’enregistre pas l’utilisateur hors du fournisseur externe. Pour connecter l’utilisateur de Facebook et empêcher la persistance une fois que l’utilisateur a déconnecté le jeton, vous pouvez ajouter le code en surbrillance suivant à la **fermeture de session** méthode dans le contrôle AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

La valeur que vous fournissez dans le `next` paramètre est l’URL à utiliser après que l’utilisateur est déconnecté de Facebook. Lorsque vous testez sur votre ordinateur local, vous fournissez le numéro de port correct pour votre site local. En production, vous fourniriez une page par défaut, par exemple, contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Récupérer les informations utilisateur qui requiert le jeton d’accès

Maintenant que vous avez stocké le jeton d’accès et installé le package C# SDK Facebook, vous pouvez les utiliser ensemble pour demander des informations utilisateur supplémentaires auprès de Facebook. Dans le **ExternalLoginConfirmation** (méthode), créez une instance de la **FacebookClient** classe en passant la valeur du jeton d’accès. Demandez la valeur de la **vérifié** propriété pour l’utilisateur authentifié actuel. Le **vérifié** propriété indique si Facebook a validé le compte par le biais d’autres moyens, comme l’envoi d’un message à un téléphone portable. Enregistrez cette valeur dans la base de données.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Vous devrez à nouveau supprimer les enregistrements dans la base de données pour l’utilisateur, ou utiliser un autre compte Facebook.

Exécutez l’application, puis inscrire le nouvel utilisateur. Examinez le **ExtraUserInformation** une table pour afficher la valeur de la propriété vérifié.

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez créé un site qui est intégré à Facebook pour l’authentification des utilisateurs et des données d’inscription. Vous avez découvert le comportement par défaut qui est configuré pour l’application web MVC 4 et comment personnaliser ce comportement par défaut.

## <a name="related-topics"></a>Rubriques connexes

- [Créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer sur Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
