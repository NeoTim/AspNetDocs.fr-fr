---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Utilisation d’un site CAPTCHA pour empêcher les bots d’utiliser votre site de ASP.NET De rasoir Web) Microsoft Docs
author: rick-anderson
description: Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés (bots) d’effectuer des tâches dans une ASP.NET Pages Web (Razor) nous ...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 65f414ae3fed5e2fa28b1e57f5327c6411a43d55
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543754"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Utilisation d’un site CAPTCHA pour empêcher les bots d’utiliser votre site ASP.NET Web Razor)

par [Microsoft](https://github.com/microsoft)

> Cet article explique comment utiliser ReCaptcha (une mesure de sécurité) pour empêcher les programmes automatisés (bots) d’effectuer des tâches dans un site Web ASP.NET Pages Web (Razor).
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment ajouter un test CAPTCHA à votre site.
> 
> Voici les caractéristiques ASP.NET introduites dans l’article :
> 
> - L’aide. `ReCaptcha`
> 
> > [!NOTE]
> > Les informations contenues dans cet article s’appliquent aux pages Web ASP.NET 1.0 et Web Pages 2.

## <a name="about-captchas"></a>À propos de CAPTCHAs

Chaque fois que vous laissez les gens s’inscrire sur votre site, ou même tout simplement entrer un nom et URL (comme pour un commentaire de blog), vous pourriez obtenir un flot de faux noms. Ceux-ci sont souvent laissés par des programmes automatisés (bots) qui essaient de laisser des URL dans chaque site Web qu’ils peuvent trouver. (Une motivation commune est d’afficher les URL des produits à vendre.)

Vous pouvez vous assurer qu’un utilisateur est une personne réelle et non un programme informatique en utilisant un *CAPTCHA* pour valider les utilisateurs lorsqu’ils s’inscrivent ou entrent leur nom et leur site. CAPTCHA signifie tout à fait automatisé test de turing public pour dire ordinateurs et humains à part. Un CAPTCHA est un test *de réponse* aux défis dans lequel l’utilisateur est invité à faire quelque chose qui est facile à faire pour une personne, mais difficile pour un programme automatisé à faire. Le type le plus commun de CAPTCHA est celui où vous voyez quelques lettres déformées et on vous demande de les taper. (La distorsion est censée rendre difficile pour les bots de déchiffrer les lettres.)

## <a name="adding-a-recaptcha-test"></a>Ajout d’un test ReCaptcha

Dans ASP.NET pages, vous pouvez `ReCaptcha` utiliser l’aide pour rendre un test CAPTCHA basé[http://recaptcha.net](http://recaptcha.net)sur le service ReCaptcha ( ). L’aide `ReCaptcha` affiche une image de deux mots déformés que les utilisateurs doivent entrer correctement avant que la page ne soit validée. La réponse de l’utilisateur est validée par le service ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Inscrivez votre site Web à[http://recaptcha.net](http://recaptcha.net)ReCaptcha.Net ( ). Lorsque vous aurez terminé l’inscription, vous obtiendrez une clé publique et une clé privée.
2. Ajoutez la ASP.NET Bibliothèque d’aide Web à votre site Web tel que décrit dans [Installing Helpers dans un site de pages Web ASP.NET,](https://go.microsoft.com/fwlink/?LinkId=252372)si vous ne l’avez pas déjà fait.
3. Si vous n’avez pas déjà un * \_fichier AppStart.cshtml,* dans le dossier racine d’un site Web créer un fichier nommé * \_AppStart.cshtml*.
4. Ajoutez les `Recaptcha` paramètres d’assistance suivants dans le * \_fichier AppStart.cshtml* : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Réglez `PrivateKey` les propriétés et les propriétés à l’aide `PublicKey` de vos propres clés publiques et privées.
6. Enregistrez le fichier * \_AppStart.cshtml* et fermez-le.
7. Dans le dossier racine d’un site Web, créer une nouvelle page nommée *Recaptcha.cshtml*.
8. Remplacer le contenu existant par les éléments suivants : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Exécutez la page *Recaptcha.cshtml* dans un navigateur. Si `PrivateKey` la valeur est valide, la page affiche le contrôle ReCaptcha et un bouton. Si vous n’aviez pas défini les clés globalement dans * \_AppStart.html*, la page afficherait une erreur. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Entrez les mots pour le test. Si vous réussissez le test ReCaptcha, vous voyez un message à cet effet. Sinon, vous voyez un message d’erreur et le contrôle ReCaptcha est redisjoué.

> [!NOTE]
> Si votre ordinateur se trouve sur un domaine qui `defaultproxy` utilise un serveur proxy, vous devrez peut-être configurer l’élément du fichier *Web.config.* L’exemple suivant montre un fichier `defaultproxy` *Web.config* avec l’élément configuré pour permettre au service ReCaptcha de fonctionner.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Personnaliser le comportement à l’échelle du site pour ASP.NET sites de pages Web](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Site de ReCaptcha](https://www.google.com/recaptcha)
