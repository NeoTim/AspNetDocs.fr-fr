---
uid: mvc/overview/getting-started/mvc-learning-sequence
title: Didacticiels et articles recommandés pour MVC | Microsoft Docs
author: Rick-Anderson
description: Cette page contient des liens vers des didacticiels ASP.NET MVC et une séquence suggérée pour les suivre.
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 8513a57a-2d45-4d6b-881c-15a01c5cbb1c
msc.legacyurl: /mvc/overview/getting-started/mvc-learning-sequence
msc.type: authoredcontent
ms.openlocfilehash: 7dc81cf09309194df4471fedfc74d4051f0fdb78
ms.sourcegitcommit: 8d34fb54e790cfba2d64097afc8276da5b22283e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85484215"
---
# <a name="mvc-recommended-tutorials-and-articles"></a>Tutoriels et articles recommandés pour MVC

par [Rick Anderson](https://twitter.com/RickAndMSFT)

<a id="pwd"></a>
## <a name="getting-started"></a>Mise en route

- [Prise en main avec ASP.NET MVC 5](introduction/getting-started.md) Cette série de 11 parties est un bon point de départ.
- [Notions de base de Pluralsight ASP.NET MVC 5](https://pluralsight.com/training/Player?author=scott-allen&amp;name=aspdotnet-mvc5-fundamentals-m1-introduction&amp;mode=live&amp;clip=0&amp;course=aspdotnet-mvc5-fundamentals) (cours vidéo)
- [Introduction à ASP.NET MVC](https://channel9.msdn.com/Series/Introduction-to-ASP-NET-MVC) par Jon Galloway et Christopher Harrison
- [Cycle de vie d’une Application ASP.NET MVC 5](lifecycle-of-an-aspnet-mvc-5-application.md) Document PDF qui représente le cycle de vie d’une application ASP.NET MVC 5.

<a id="con"></a>
## <a name="working-with-data"></a>Utilisation de données

- [Prise en main avec EF 6 Code First à l’aide de MVC 5](getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) La série primée de Tom Dykstra explore en profondeur EF.

<a id="wj"></a>
## <a name="security"></a>Sécurité

- [Créer une application ASP.NET MVC avec auth et SQL DB et la déployer sur Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Ce didacticiel populaire vous guide tout au long de la création d’une application simple et de l’ajout d’appartenances et de rôles.
- [Créer une application ASP.NET MVC 5 avec l’authentification Facebook, Twitter, LinkedIn et Google OAuth2](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ce didacticiel vous montre comment créer une application Web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2,0 avec les informations d’identification d’un fournisseur d’authentification externe, tel que Facebook, Twitter, LinkedIn, Microsoft ou Google.
- [Créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) Tout d’abord, dans une série sur l’identité, comprend du code pour [renvoyer un lien de confirmation](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md#rsend).
- [Application ASP.NET MVC 5 avec SMS et E-mail d’authentification à deux facteurs](../security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) Seconde sur les séries d’identité.
- [Bonnes pratiques pour le déploiement des mots de passe et d’autres données sensibles sur ASP.NET et Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)
- [Authentification à deux facteurs à l’aide de SMS et e-mail avec ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) `isPersistent` et le cookie de sécurité, code pour demander à un utilisateur de disposer d’un compte de messagerie validé avant qu’il puisse se connecter, comment SignInManager vérifie la spécification 2FA, et bien plus encore.
- [Confirmation de compte et récupération de mot de passe avec ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Fournit des détails sur l’identité introuvable dans [créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe](../security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) , par exemple comment permettre aux utilisateurs de réinitialiser leur mot de passe oublié.

<a id="da"></a>
## <a name="azure"></a>Azure

- [Créer une application web ASP.net dans Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/) Didacticiel simple et rapide pour le déploiement sur Azure.
- [Créer une application ASP.NET MVC avec auth et SQL DB et la déployer sur Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)

<a id="perf"></a>
## <a name="performance-and-debugging"></a>Performances et débogage

- [Profiler et déboguer votre application ASP.NET MVC avec Glimpse](../performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse.md)

## <a name="aspnet-mvc-dropdownlistfor-with-selectlistitem"></a>ASP.NET MVC DropDownListFor avec SelectListItem

Lorsque vous utilisez le <xref:System.Web.Mvc.Html.SelectExtensions.DropDownListFor%2A> programme d’assistance et que vous lui passez la collection de `SelectListItem` à partir de laquelle il est rempli, le `DropdownListFor` modifie la collection passée après qu’il a été appelé. `DropdownListFor`remplace les `SelectListItems` propriétés sélectionnées par les propriétés sélectionnées par la liste déroulante. Cela provoque un comportement inattendu.

<xref:System.Web.Mvc.Html.SelectExtensions.DropDownListFor%2A>, <xref:System.Web.Mvc.Html.SelectExtensions.DropDownList%2A> ,, <xref:System.Web.Mvc.Html.SelectExtensions.EnumDropDownListFor%2A> <xref:System.Web.Mvc.Html.SelectExtensions.ListBox%2A> Et <xref:System.Web.Mvc.Html.SelectExtensions.ListBoxFor%2A> mettent à jour la propriété sélectionnée de tout `IEnumerable<SelectListItem>` passé ou trouvé dans ViewData.

La solution de contournement consiste à créer des énumérables distincts, contenant `SelectListItem` des instances distinctes, pour chaque propriété du modèle.

Pour plus d'informations, consultez la rubrique

* [DropdownListFor modifie la collection SelectListItem qui lui est transmise](http://web.archive.org/web/20140902031437/http://aspnetwebstack.codeplex.com/workitem/1913)
* [GetSelectListWithDefaultValue modifie IEnumerable <SelectListItem> SelectList](https://github.com/aspnet/AspNetWebStack/issues/271)