---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Créez l’application MVC 5 avec Facebook, Twitter, LinkedIn et Google OAuth2 Sign-on (C) Microsoft Docs
author: Rick-Anderson
description: Ce tutoriel vous montre comment construire une application web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2.0 avec des informations d’identification d’un authenti externe...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676320"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Créer une application ASP.NET MVC 5 avec authentification OAuth2 Facebook, Twitter, LinkedIn et Google (C#)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce tutoriel vous montre comment construire une application web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide [d’OAuth 2.0](http://oauth.net/2/) avec des informations d’identification d’un fournisseur d’authentification externe, tels que Facebook, Twitter, LinkedIn, Microsoft ou Google. Pour plus de simplicité, ce tutoriel se concentre sur le travail avec les informations d’identification de Facebook et Google.
> 
> L’activation de ces informations d’identification sur vos sites Web offre un avantage significatif car des millions d’utilisateurs ont déjà des comptes auprès de ces fournisseurs externes. Ces utilisateurs peuvent être plus enclins à s’inscrire à votre site s’ils n’ont pas à créer et à se souvenir d’un nouvel ensemble d’informations d’identification.
> 
> Voir aussi [ASP.NET application MVC 5 avec SMS et e-mail Authentification à deux facteurs](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Le tutoriel montre également comment ajouter des données de profil pour l’utilisateur, et comment utiliser l’API d’adhésion pour ajouter des rôles. Ce tutoriel a été écrit par [Rick](https://blogs.msdn.com/rickAndy) [@RickAndMSFT](https://twitter.com/RickAndMSFT) Anderson ( S’il vous plaît suivez-moi sur Twitter: ).

<a id="start"></a>
## <a name="getting-started"></a>Mise en route

Commencez par installer et exécuter [Visual Studio Express 2013 pour Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou Visual Studio [2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installez Visual Studio [2013 Mise à jour 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou plus. Pour de l’aide avec Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, et plus encore, voir ce [projet d’exemple](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Vous devez installer Visual Studio [2013 Mise à jour 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou plus pour utiliser Google OAuth 2 et de débouger localement sans avertissements SSL.

Cliquez sur **New Project** à partir de la page **Démarrer,** ou vous pouvez utiliser le menu et sélectionner **le fichier**, puis new **Project**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Création de votre première application

Cliquez sur **New Project**, puis sélectionnez Visual **C sur** la gauche, puis sur le **Web,** puis sélectionnez **ASP.NET application Web**. Nommez votre projet "MvcAuth" et cliquez **ensuite sur OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Dans le dialogue **du projet New ASP.NET,** cliquez sur **MVC**. Si l’authentification **n’est**pas un compte utilisateur individuel, cliquez sur le bouton **Modifier l’authentification** et sélectionnez **les comptes utilisateur individuels**. En vérifiant **Host dans le cloud**, l’application sera très facile à accueillir en Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Si vous avez sélectionné **Host dans le cloud,** complétez le dialogue configuré.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Utilisez NuGet pour mettre à jour le dernier middleware OWIN

Utilisez le gestionnaire de paquets NuGet pour mettre à jour le [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Sélectionnez **les mises à jour** dans le menu gauche. Vous pouvez cliquer sur le bouton **Mise à jour Tous** ou vous pouvez rechercher uniquement des paquets OWIN (indiqués dans l’image suivante) :

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Dans l’image ci-dessous, seuls les paquets OWIN sont affichés :

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Depuis la console Package Manager (PMC), vous pouvez entrer la `Update-Package` commande, qui mettra à jour tous les paquets.

Appuyez sur **F5** ou **Ctrl-F5** pour exécuter l’application. Dans l’image ci-dessous, le numéro de port est 1234. Lorsque vous exécutez l’application, vous verrez un numéro de port différent.

Selon la taille de la fenêtre de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour voir la **maison**, **Environ**, **Contact**, **Inscrivez-vous** et **Connectez-vous dans** les liens.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Mise en place de SSL dans le projet

Pour vous connecter à des fournisseurs d’authentification comme Google et Facebook, vous devrez configurer IIS-Express pour utiliser SSL. Il est important de continuer à utiliser SSL après la connexion et de ne pas repoinuer sur HTTP, votre cookie de connexion est tout aussi secret que votre nom d’utilisateur et mot de passe, et sans utiliser SSL vous l’envoyez en texte clair sur le fil. En outre, vous avez déjà pris le temps d’effectuer la poignée de main et sécuriser le canal (qui est la majeure partie de ce qui rend HTTPS plus lent que HTTPS) avant le pipeline MVC est exécuté, afin de rediriger vers HTTP après que vous êtes connecté ne fera pas la demande actuelle ou les demandes futures beaucoup plus rapide.

1. Dans **Solution Explorer**, cliquez sur le projet **MvcAuth.**
2. Appuyez sur la clé F4 pour afficher les propriétés du projet. Alternativement, à partir du menu **View,** vous pouvez sélectionner **Properties Window**.
3. Changer **SSL Enabled** to True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copiez l’URL SSL `https://localhost:44300/` (qui sera sauf si vous avez créé d’autres projets SSL).
5. Dans **Solution Explorer**, cliquez à droite sur le projet **MvcAuth** et sélectionnez **propriétés**.
6. Sélectionnez l’onglet **Web,** puis collez l’URL SSL dans la boîte **d’Url de projet.** Enregistrer le fichier (Ctl-S). Vous aurez besoin de cette URL pour configurer les applications d’authentification Facebook et Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Ajoutez l’attribut [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) au `Home` contrôleur pour exiger que toutes les demandes doivent utiliser HTTPS. Une approche plus sûre consiste à ajouter le filtre [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) à l’application. Voir la &quot;section Protéger l’application avec SSL et l’attribut d’autorisation&quot; dans mon tutoriel Créer une application ASP.NET [MVC avec auth et SQL DB et déployer à Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Une partie du contrôleur à domicile est indiquée ci-dessous.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Appuyez sur Ctrl+F5 pour exécuter l’application. Si vous avez installé le certificat dans le passé, vous pouvez sauter le reste de cette section et sauter à [la création d’une application Google pour OAuth 2 et la connexion de l’application au projet](#goog), sinon, suivez les instructions pour faire confiance au certificat auto-signé qu’IIS Express a généré.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lisez le dialogue **d’avertissement de sécurité** et cliquez ensuite sur **Oui** si vous voulez installer le certificat représentant localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE affiche la *page d’accueil* sans avertissement SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome accepte également le certificat et affichera le contenu HTTPS sans avertissement. Firefox utilise son propre magasin de certificats, de sorte qu’il affichera un avertissement. Pour notre application, vous pouvez cliquer en toute sécurité **sur je comprends les risques**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Création d’une application Google pour OAuth 2 et connexion de l’application au projet

> [!WARNING]
> Pour les instructions actuelles de Google OAuth, voir [Configuring Google authentification dans ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Accédez à la [Console développeur de Google](https://console.developers.google.com/).
2. Si vous n’avez pas créé de projet auparavant, sélectionnez **les informations d’identification** dans l’onglet gauche, puis sélectionnez **Créer**.
3. Dans l’onglet gauche, cliquez sur **Credentials**.
4. Cliquez **sur Créer des informations d’identification** puis **OAuth id client**. 

    1. Dans le dialogue **Create Client ID,** conservez l’application **Web** par défaut pour le type d’application.
    2. Définissez les origines **JavaScript autorisées** à l’URL SSL que vous avez utilisée ci-dessus (sauf`https://localhost:44300/` si vous avez créé d’autres projets SSL)
    3. Définissez **l’URI rediriger autorisé** vers :  
         `https://localhost:44300/signin-google`
5. Cliquez sur l’élément du menu d’écran OAuth Consent, puis réglez votre adresse e-mail et votre nom de produit. Lorsque vous avez terminé le formulaire cliquez sur **Enregistrer**.
6. Cliquez sur l’élément menu de la Bibliothèque, **recherchez l’API googleMD,** cliquez dessus, puis appuyez sur Enable.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   L’image ci-dessous montre les API activées.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Du gestionnaire de l’API de Google, visitez **l’onglet Credentials** pour obtenir **l’ID client**. Téléchargez pour enregistrer un fichier JSON avec des secrets d’application. Copiez et collez le **ClientId** `UseGoogleAuthentication` et **ClientSecret** dans la méthode trouvée dans le fichier *Startup.Auth.cs* dans le dossier *App_Start.* Les valeurs **ClientId** et **ClientSecret** indiquées ci-dessous sont des échantillons et ne fonctionnent pas.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sécurité - Ne stockez jamais de données sensibles dans votre code source. Le compte et les informations d’identification sont ajoutés au code ci-dessus pour garder l’échantillon simple. Voir [les meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles à ASP.NET et Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Appuyez sur **CTRL-F5** pour construire et exécuter l’application. Cliquez sur le lien **Ouvrir une session** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Sous **l’utilisation d’un autre service pour vous connecter,** cliquez sur **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Si vous manquez l’une des étapes ci-dessus, vous obtiendrez une erreur HTTP 401. Revérifier vos pas ci-dessus. Si vous manquez un paramètre requis (par exemple **le nom du produit),** ajoutez l’article manquant et enregistrez; il peut prendre quelques minutes pour que l’authentification fonctionne.
10. Vous serez redirigé vers le site Google où vous entrerez vos informations d’identification.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Après avoir entré vos informations d’identification, vous êtes invité à accorder des autorisations pour l’application Web que vous venez de créer : 
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Cliquez sur **Accepter**. Vous serez désormais redirigé vers la page **Registre** de l’application MvcAuth où vous pouvez enregistrer votre compte Google. Vous avez la possibilité de changer le nom d'inscription local utilisé pour votre compte Gmail, mais, en règle générale, les utilisateurs préfèrent conserver l'alias de messagerie par défaut (c'est-à-dire celui que vous avez utilisé pour l'authentification). Cliquez sur **S'inscrire**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Création de l’application dans Facebook et connexion de l’application au projet

> [!WARNING]
> Pour les instructions actuelles d’authentification Facebook OAuth2, voir [Configuring Facebook authentification](/aspnet/core/security/authentication/social/facebook-logins)

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examiner les données d’adhésion

Dans le menu **View,** cliquez sur **Server Explorer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Expand **DefaultConnection (MvcAuth)**, expand **Tables**, cliquez à droite **AspNetUsers** et cliquez sur **Show Table Data**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![données de table aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Ajout de données de profil à la classe utilisateur

Dans cette section, vous ajouterez la date de naissance et la ville natale aux données utilisateur lors de l’enregistrement, comme indiqué dans l’image suivante.

![reg avec la ville natale et Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Ouvrez le fichier *Models-IdentityModels.cs* et ajoutez la date de naissance et les propriétés de la ville natale :

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Ouvrez le fichier *Models-AccountViewModels.cs* et la date `ExternalLoginConfirmationViewModel`de naissance définie et les propriétés de la ville d’origine dans .

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Ouvrez le fichier *Controllers-AccountController.cs* et ajoutez du `ExternalLoginConfirmation` code pour la date de naissance et la ville d’origine dans la méthode d’action comme indiqué :

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Ajoutez la date de naissance et la ville natale au fichier *Views-Account-ExternalLoginConfirmation.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Supprimez la base de données d’adhésion afin que vous puissiez à nouveau enregistrer votre compte Facebook avec votre application et vérifier que vous pouvez ajouter la nouvelle date de naissance et les informations de profil de ville d’origine.

De **Solution Explorer**, cliquez sur **l’icône Afficher tous les fichiers,** puis cliquez à droite *Ajouter\_Data-aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* et cliquez sur **Supprimer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

À partir du menu **Tools,** cliquez sur **NuGet Package Manger**, puis cliquez sur **Package Manager Console** (PMC). Entrez les commandes suivantes dans le PMC.

1. Activer les migrations
2. Add-Migration Init
3. Mise à jour-Base de données

Exécutez l’application et utilisez FaceBook et Google pour vous connecter et enregistrer certains utilisateurs.

## <a name="examine-the-membership-data"></a>Examiner les données d’adhésion

Dans le menu **View,** cliquez sur **Server Explorer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Cliquez à droite **AspNetUsers** et cliquez sur **Afficher les données de la table**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Les `HomeTown` `BirthDate` champs et les champs sont indiqués ci-dessous.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Vous vous connectez à votre application et vous connectez-vous avec un autre compte

Si vous vous connectez à votre application avec Facebook, puis vous vous déconnectez et essayez de vous connecter à nouveau avec un autre compte Facebook (à l’aide du même navigateur), vous serez immédiatement connecté au compte Facebook précédent que vous avez utilisé. Afin d’utiliser un autre compte, vous devez naviguer vers Facebook et vous connecter à Facebook. La même règle s’applique à tout autre fournisseur d’authentification de la 3e partie. Alternativement, vous pouvez vous connecter avec un autre compte en utilisant un navigateur différent.

## <a name="next-steps"></a>Étapes suivantes

Voir [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions. Consultez les boutons de connexion sociale de Jerrie pour ASP.NET MVC 5 pour obtenir des boutons de connexion sociale.

Suivez mon tutoriel [Créer une application MVC ASP.NET avec auth et SQL DB et déployer à Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), qui continue ce tutoriel et montre ce qui suit:

1. Comment déployer votre application sur Azure.
2. Comment sécuriser votre application avec des rôles.
3. Comment sécuriser votre application avec les [Filtres RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) et [autoriser.](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)
4. Comment utiliser l’API d’adhésion pour ajouter des utilisateurs et des rôles.

S’il vous plaît laisser des commentaires sur la façon dont vous avez aimé ce tutoriel et ce que nous pourrions améliorer. Vous pouvez également demander de nouveaux sujets à [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Vous pouvez même demander et voter sur de nouvelles fonctionnalités à ajouter à ASP.NET. Par exemple, vous pouvez voter pour un outil pour créer et gérer les [utilisateurs et les rôles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Pour une bonne explication du fonctionnement de ASP.NET Services d’authentification externe, consultez [les services d’authentification externe](https://asp.net/web-api/overview/security/external-authentication-services)de Robert McMurray . L’article de Robert entre également dans les détails en permettant l’authentification microsoft et Twitter. L’excellent [tutoriel EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) de Tom Dykstra montre comment travailler avec le Cadre d’entités.
