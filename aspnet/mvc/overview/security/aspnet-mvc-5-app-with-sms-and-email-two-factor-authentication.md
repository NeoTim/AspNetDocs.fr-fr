---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Application ASP.NET MVC 5 avec SMS et e-mail d’authentification à deux facteurs | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application web de ASP.NET MVC 5 avec authentification à deux facteurs. Vous devez effectuer un ASP.NET MVC 5 sécurisée de créer une application web avec...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 25d21efaf2f01ee1c162408a3caf699ac818aaa7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59384954"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Application ASP.NET MVC 5 avec authentification à deux facteurs par SMS et e-mail

par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ce didacticiel vous montre comment créer une application web de ASP.NET MVC 5 avec authentification à deux facteurs. Vous devez effectuer [créer une application web de ASP.NET MVC 5 sécurisée avec connexion, réinitialisation de confirmation et le mot de passe de messagerie](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) avant de continuer. Vous pouvez télécharger l’application terminée [ici](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Le téléchargement contient les helpers de débogage qui vous permettent de tester une confirmation par e-mail et par SMS sans configurer un fournisseur de messagerie ou de SMS.
> 
> Ce didacticiel a été écrit par [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter : [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


- [Créer une application ASP.NET MVC](#createMvc)
- [Configurer SMS pour l’authentification à deux facteurs](#SMS)
- [Activer l’authentification à deux facteurs](#enable2)
- [Ressources supplémentaires](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Créer une application ASP.NET MVC

Démarrez en installant et en cours d’exécution [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure.

> [!NOTE]
> Avertissement : Vous devez effectuer [créer une application web de ASP.NET MVC 5 sécurisée avec connexion, réinitialisation de confirmation et le mot de passe de messagerie](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) avant de continuer. Vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure pour suivre ce didacticiel.


1. Créez un projet Web ASP.NET et sélectionnez le modèle MVC. Web Forms prend également en charge ASP.NET Identity : vous pouvez donc suivre des étapes similaires dans une application Web Forms.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Laissez l’authentification par défaut à **Comptes d’utilisateur individuels**. Si vous souhaitez héberger l’application dans Azure, laissez la case à cocher activée. Plus loin dans ce didacticiel, nous déploierons sur Azure. Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Définir le [projet pour utiliser SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Configurer SMS pour l’authentification à deux facteurs

Ce didacticiel fournit des instructions pour l’utilisation de Twilio ou ASPSMS, mais vous pouvez utiliser n’importe quel autre fournisseur SMS.

1. **Création d’un compte d’utilisateur avec un fournisseur SMS**  
  
   Créer un [Twilio](https://www.twilio.com/try-twilio) ou un [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) compte.
2. **Installation des packages supplémentaires ou l’ajout de références de service**  
  
   Twilio :  
   Dans la Console du Gestionnaire de Package, entrez la commande suivante :  
    `Install-Package Twilio`  
  
   ASPSMS :  
   La référence de service suivante doit être ajoutée :  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Adresse :  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Espace de noms :  
    `ASPSMSX2`
3. **Déterminer les informations d’identification de l’utilisateur du fournisseur SMS**  
  
   Twilio :  
   À partir de la **tableau de bord** onglet de votre compte Twilio, copie le **SID de compte** et **du jeton d’authentification**.  
  
   ASPSMS :  
   À partir des paramètres de votre compte, accédez à **Userkey** et copiez-le avec votre définis par l’utilisateur **mot de passe**.  
  
   Nous stockons ultérieurement ces valeurs dans le *web.config* fichier parmi les clés `"SMSAccountIdentification"` et `"SMSAccountPassword"` .
4. **Spécifiant l’ID d’expéditeur / donneur d’ordre**  
  
   Twilio :  
   À partir de la **numéros** onglet, copiez votre numéro de téléphone Twilio.  
  
   ASPSMS :  
   Dans le **des expéditeurs de déverrouiller** déverrouiller un ou plusieurs expéditeurs de Menu, ou choisissez un expéditeur d’alphanumériques (non pris en charge par tous les réseaux).  
  
   Nous stockons ultérieurement cette valeur dans le *web.config* fichier au sein de la clé `"SMSAccountFrom"` .
5. **Transfert des informations d’identification du fournisseur SMS dans l’application**  
  
   Fournir les informations d’identification et le numéro de téléphone de l’expéditeur à l’application. Pour simplifier les choses, nous allons stocker ces valeurs dans le *web.config* fichier. Lors du déploiement dans Azure, nous pouvons stocker les valeurs de manière sécurisée dans le **paramètres de l’application** section sur le site web de l’onglet Configurer. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont ajoutées au code ci-dessus pour que l’exemple reste simple. Consultez [les meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implémentation de transfert de données vers le fournisseur SMS**  
  
   Configurer le `SmsService` classe dans le *application\_Start\IdentityConfig.cs* fichier.  
  
   Selon le fournisseur SMS utilisé activer soit le **Twilio** ou **ASPSMS** section : 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Mise à jour le *Views\Manage\Index.cshtml* vue Razor : (Remarque : ne pas simplement supprimer les commentaires dans le code existant, utilisez le code ci-dessous.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Vérifiez le `EnableTwoFactorAuthentication` et `DisableTwoFactorAuthentication` méthodes d’action dans le `ManageController` ont le[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribut :  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Exécutez l’application et connectez-vous avec le compte que vous avez enregistré précédemment.
10. Cliquez sur votre ID d’utilisateur, ce qui active le `Index` méthode d’action dans `Manage` contrôleur.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Cliquez sur Ajouter.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. Le `AddPhoneNumber` méthode d’action affiche une boîte de dialogue pour entrer un numéro de téléphone qui permettre recevoir des messages SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. En quelques secondes, vous obtiendrez un message texte avec le code de vérification. Entrez-le, puis appuyez sur **envoyer**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. La vue de gérer affiche que votre numéro de téléphone a été ajouté.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Activer l’authentification à deux facteurs

Dans l’application du modèle généré, vous devez utiliser l’interface utilisateur pour activer l’authentification à deux facteurs (2FA). Pour activer 2FA, cliquez sur votre ID d’utilisateur (alias de messagerie) dans la barre de navigation.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Cliquez sur Activer 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Déconnexion, puis reconnectez-vous. Si vous avez activé la messagerie (voir mon [didacticiel précédent](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), vous pouvez sélectionner le SMS ou e-mail 2fa.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

La page de codes de vérification s’affiche où vous pouvez entrer le code (à partir de SMS ou e-mail).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

En cliquant sur le **mémoriser ce navigateur** case à cocher vous exempter de devoir utiliser 2FA pour vous connecter lorsque vous utilisez le navigateur et l’appareil où vous avez coché la case. Tant que les utilisateurs malveillants ne peuvent pas accéder à votre appareil, l’activation à 2 facteurs et en cliquant sur le **mémoriser ce navigateur** vous fournira un accès mot de passe une seule étape pratique, tout en conservant une protection forte 2FA pour tous les accès à partir d’appareils non approuvé. Vous pouvez effectuer cela sur n’importe quel appareil privé que vous utilisez régulièrement.

Ce didacticiel fournit une présentation rapide de permettre à 2 facteurs sur une application ASP.NET MVC. Mon didacticiel [authentification à deux facteurs à l’aide de SMS et e-mail avec ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) détaille le code-behind de l’exemple.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Authentification à deux facteurs à l’aide de SMS et e-mail avec ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) décrit en détail sur l’authentification à deux facteurs
- [Ressources recommandées pour des liens vers ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmation de compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Décrit plus en détails la confirmation de compte et la récupération de mot de passe.
- [Application MVC 5 avec connexion Facebook, Twitter, LinkedIn et Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ce tutoriel vous montre comment écrire une application ASP.NET MVC 5 avec une autorisation Facebook et Google OAuth2. Il montre également comment ajouter des données supplémentaires à la base de données Identity.
- [Déployer une application ASP.NET MVC sécurisée avec appartenance, OAuth et base de données SQL sur Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ce tutoriel ajoute le déploiement Azure, la sécurisation de votre application avec des rôles, l’utilisation de l’API d’appartenance pour ajouter des utilisateurs et des rôles, et des fonctionnalités de sécurité supplémentaires.
- [Création d’une application Google pour oauth2 et de connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Création de l’application dans Facebook et la connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuration de SSL dans le projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [Comment configurer votre environnement de développement c# et ASP.NET MVC](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
