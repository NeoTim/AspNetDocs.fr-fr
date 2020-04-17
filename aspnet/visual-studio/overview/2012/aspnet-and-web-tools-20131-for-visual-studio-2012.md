---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notes de sortie pour ASP.NET et Web Tools 2013.1 pour Visual Studio 2012 ( Microsoft Docs
author: rick-anderson
description: Ce document décrit la sortie de ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d4aced4e77a150d52358c2d2513ff79e6594a9de
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543572"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET et Web Tools 2013.1 pour Visual Studio 2012 - Notes de publication

par [Microsoft](https://github.com/microsoft)

> Ce document décrit la sortie de ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.

## <a name="contents"></a>Contents

- [Notes d’installation](#install)
- [Exigences logicielles](#requirements)
- Nouvelles fonctionnalités dans ASP.NET et Web Tools 2013.1 pour Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Modèles](#templates)

        - [modèle MVC 5 ASP.NET](#mvc5template)
        - [modèle web API 2 ASP.NET](#apitemplate)
        - [Modèles d’objets](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET échafaudage](#scaffold)
    - [Éditeur de rasoir](#razor)
    - [NuGet 2.7](#nuget)
- Problèmes connus et changements de rupture

    - [ASP.NET échafaudage](#issuescaffolding)

        - [MVC et Web API Échafaudage - HTTP 404, erreur non trouvée](#404issue)
        - [Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un article échafaudé](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Affichage du fichier cshtml avec Browse With ou F5 provoque une erreur de serveur](#browseissue)
        - [Url Rewrite et Tilde(MD)](#rewriteissue)
    - [Modèles](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Notes d'installation

[Installer](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Configuration logicielle

Vous devez avoir Visual Studio 2012 ou Visual Studio Express 2012 pour le Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nouvelles fonctionnalités dans ASP.NET et Web Tools 2013.1 pour Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Lorsque vous échafaudez les contrôleurs et les vues MVC 5, le balisage pour les vues utilise [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Modèles

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>modèle MVC 5 ASP.NET

Nous avons ajouté un nouveau modèle MVC 5. Il fait référence aux derniers paquets MVC 5 NuGet, et vous pouvez utiliser des échafaudages pour ajouter des contrôleurs et des vues.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>modèle web API 2 ASP.NET

Nous avons ajouté un nouveau modèle Web API 2. Il fait référence aux derniers paquets NuGet Web API 2, et vous pouvez utiliser des échafaudages pour ajouter des contrôleurs et des vues.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Modèles d'élément

Nous avons ajouté de nouveaux modèles d’éléments pour les vues MVC 5, les pages Web (Razor 3) et les contrôleurs Web API 2. Ils installent les paquets NuGet connexes au projet tout en ajoutant de nouveaux éléments.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Lorsque vous échafaudez un contrôleur MVC ou Web API à l’aide d’Entity Framework, nous utilisons le Cadre 6. Pour plus d’informations sur le cadre de l’entité, voir [l’histoire de la version cadre de l’entité](https://msdn.com/data/jj574253).

Vous pouvez également télécharger et installer les outils Du cadre d’entité 6 pour Visual Studio 2012. Voir le [cadre Get Entity](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET échafaudage

ASP.NET Scaffolding est un cadre de génération de code pour les applications Web ASP.NET. Il est facile d’ajouter du code de plaque chauffante à votre projet qui interagit avec un modèle de données.

Dans les versions précédentes de Visual Studio, les échafaudages se limitaient à ASP.NET projets MVC. Avec cette mise à jour, vous pouvez maintenant utiliser des échafaudages pour n’importe quel projet ASP.NET, y compris les formulaires Web. Cette mise à jour ne prend pas en charge la génération de pages pour un projet Web Forms, mais vous pouvez toujours utiliser des échafaudages avec des formulaires Web en ajoutant des dépendances MVC au projet. La prise en charge de la génération de pages pour les formulaires Web sera ajoutée dans une prochaine mise à jour.

Lors de l’utilisation d’échafaudages, nous nous assurons que toutes les dépendances requises sont installées dans le projet. Par exemple, si vous commencez par un projet ASP.NET Web Forms, puis utilisez des échafaudages pour ajouter un contrôleur d’API Web, les paquets et références NuGet requis sont ajoutés automatiquement à votre projet.

Pour ajouter des échafaudages MVC à un projet Web Forms, ajoutez un **nouvel article échafaudé** et sélectionnez **les dépendances MVC 5** dans la fenêtre de dialogue. Il existe deux options pour l’échafaudage MVC; Minimal et complet. Si vous sélectionnez Minimal, seuls les paquets NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option Complète, les dépendances minimales sont ajoutées, ainsi que les fichiers de contenu requis pour un projet MVC.

Le support pour les contrôleurs async d’échafaudage utilise les nouvelles fonctionnalités async de Entity Framework 6.

Pour plus d’informations et de tutoriels, voir [ASP.NET Aperçu de l’échafaudage](../2013/aspnet-scaffolding-overview.md). Ces tutoriels montrent des échafaudages avec Visual Studio 2013, mais ils sont également applicables à ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Éditeur de rasoir

Avec cette mise à jour, Visual Studio 2012 prend désormais en charge l’outillage/édition de Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 comprend un riche ensemble de nouvelles fonctionnalités qui sont décrites en détail à [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Cette version de NuGet élimine la nécessité pour les utilisateurs de permettre explicitement à NuGet de restaurer les paquets manquants. Lors de l’installation de NuGet 2.7, les utilisateurs consentent implicitement à restaurer automatiquement les paquets manquants. Les utilisateurs peuvent se retirer explicitement de la restauration du paquet via les paramètres NuGet dans Visual Studio. Ce changement simplifie le fonctionnement de la restauration des emballages.

## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et changements de rupture

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET échafaudage

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC et Web API Échafaudage - HTTP 404, erreur non trouvée

Si vous rencontrez une erreur lors de l’ajout d’un élément échafaudé à un projet, il est possible que votre projet soit laissé dans un état incohérent. Certaines des modifications apportées à l’échafaudage seront annulées, mais d’autres changements, comme les paquets NuGet installés, ne seront pas annulés. Si les modifications de configuration de routage sont annulées, les utilisateurs recevront une erreur HTTP 404 lors de la navigation vers des éléments échafaudés.

Pour corriger cette erreur pour MVC, ajoutez un nouvel article échafaudé et sélectionnez les dépendances MVC 5 (minimales ou complètes). Ce processus ajoutera toutes les modifications requises à votre projet.

Pour corriger cette erreur pour l’API Web :

1. Ajoutez la classe WebApiConfig suivante à votre projet.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configurez WebApiConfig.Register\_dans la méthode De démarrage d’application dans Global.asax comme suit :

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un article échafaudé

Si Visual Studio Express 2012 pour Le Web cesse de fonctionner après l’ajout d’un élément échafaudé avec Entity Framework (comme Web API 2 Controller avec des actions, en utilisant Entity Framework), il est possible que Visual Studio Express n’ait pas chargé l’image native d’un assemblage dépendant de System.Web.Extensions.

Pour corriger ce problème, configurez Visual Studio Express pour travailler avec l’image MSIL de System.Web.Extensions :

1. Ouvrez l’invite de commande en mode Administrateur.
2. Rendez-vous à %ProgramFiles% -Microsoft Visual Studio 11.0-Common7-IDE ou %ProgramFiles(x86)% -Microsoft Visual Studio 11.0-Common7-IDE (pour 64 bits Windows).
3. Ouvrez VWDExpress.exe.config dans un éditeur de texte.
4. Ajoutez la ligne &lt;suivante&gt;/&lt;sous&gt; l’élément de temps d’exécution de configuration :  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Redémarrez Visual Studio Express 2012 pour le Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>Affichage du fichier cshtml avec Browse With ou F5 provoque une erreur de serveur

Lorsque vous créez un projet MVC 5 dans Visual Studio 2012 (ou ou ouvrez dans Visual Studio 2012 un projet MVC 5 qui a été créé dans Visual Studio 2013) et tentez de visualiser un fichier cshtml en utilisant Browse With ou F5, vous recevrez une erreur qui indique **l’erreur du serveur dans l’application ' /.** Le serveur tente de naviguer vers`http://localhost:XXXX/Views/../XXXX.cshtml`

Pour résoudre ce problème, modifiez le paramètre **Start Action** dans votre projet en **Page Spécifique**. Vous n’avez pas besoin de fournir une valeur pour la page.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Après avoir apporté ce changement, la sélection de F5 navigue jusqu’à la racine de votre application (`http://localhost:XXXX`). Ce comportement n’est pas le même que le comportement des projets MVC 5 dans Visual Studio 2013, où le paramètre De la **Page actuelle** lance la page ouverte.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Url Rewrite et Tilde(MD)

Après la mise à niveau vers ASP.NET Razor 3 ou ASP.NET MVC 5, la notation tilde (MD) peut ne plus fonctionner correctement si vous utilisez des réécritures d’URL. La réécriture de l’URL affecte la notation &lt;tilde (MD) dans les éléments HTML tels que&gt;A/ &lt;, SCRIPT/&gt;, &lt;LINK/&gt;, et par conséquent le tilde ne cartes plus à l’annuaire racine.

Par exemple, si vous réécrivez les demandes **de asp.net/content** &lt;à **asp.net,** l’attribut href&gt; dans A href"/content/"/ se résout à **/contenu/contenu/** au lieu de **/**. Pour supprimer ce changement, vous pouvez définir le contexte **iÉlRécrit DE l’IIS\_** à faux dans chaque page Web ou dans **l’application\_BeginRequest** dans Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Modèles

Lorsque vous créez ASP.NET projets MVC avec Visual Studio 2012 sur Windows 8.1 ou Windows Server 2012 R2, Visual Studio affiche un message d’erreur qui indique "Configuring Web [url] pour ASP.NET 4.5 a échoué."

![erreur de configuration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Vous voyez cette erreur car Visual Studio 2012 ne permet pas la ASP.NET fonction 4.5 lorsqu’elle est installée sur ces versions de Windows. Pour activer ASP.NET 4.5, effectuez les étapes décrites dans les [fonctionnalités De Turn Windows sur ou en dehors](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![activer ou désactiver les fonctionnalités Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternativement, vous pouvez activer ASP.NET 4.5 à travers la ligne de commande.

1. Ouvrez l’invite de commande en mode Administrateur.
2. Exécutez la commande suivante pour activer ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
