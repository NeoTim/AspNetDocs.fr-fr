---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Présentation d’ASP.NET AJAX localisation | Microsoft Docs
author: scottcate
description: La localisation est le processus de conception et l’intégration de la prise en charge pour une langue spécifique et une culture dans une application ou un composant d’application. Le Mic...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 86cbf150708f1db711b40ccbc25345afeb3e542a
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031436"
---
<a name="understanding-aspnet-ajax-localization"></a>Présentation de la localisation d’ASP.NET AJAX
====================
par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> La localisation est le processus de conception et l’intégration de la prise en charge pour une langue spécifique et une culture dans une application ou un composant d’application. La plateforme Microsoft ASP.NET fournit la prise en charge complète pour la localisation pour les applications ASP.NET standards en intégrant le modèle de localisation .NET standard ; l’infrastructure AJAX de Microsoft utilisent le modèle intégré pour prendre en charge les divers scénarios dans lesquels la localisation peut être effectuée.


## <a name="introduction"></a>Introduction

Technologie de Microsoft ASP.NET offre un modèle de programmation orientée objet et événementiel et il unifie les avantages du code compilé. Toutefois, son modèle de traitement côté serveur a plusieurs inconvénients inhérents à la technologie, bon nombre d'entre elles peuvent être adressés par les nouvelles fonctionnalités incluses dans l’espace de noms System.Web.Extensions, qui encapsule les Services AJAX de Microsoft dans le .NET Framework 3.5. Ces extensions permettent de nombreuses fonctionnalités de client riche, précédemment disponibles en tant que partie des Extensions ASP.NET 2.0 AJAX, mais faisant désormais partie de la bibliothèque de classes de Base de Framework. Contrôles et fonctionnalités dans cet espace de noms incluent le rendu partiel de pages sans nécessiter une actualisation de la page entière, la possibilité d’accéder aux Services Web via le script client (y compris les API de profilage ASP.NET) et une API côté client étendue conçu pour mettre en miroir de nombreux les schémas de contrôle indiqués dans l’ensemble du contrôle côté serveur ASP.NET.

Ce livre blanc examine les fonctionnalités de localisation présentes dans l’infrastructure AJAX de Microsoft et le Script Microsoft AJAX Library, dans le contexte des besoins de l’entreprise pour la prise en charge de la localisation et à la révision déjà intégré la prise en charge pour la localisation de site web applications fournies par le .NET Framework. La bibliothèque de scripts Microsoft AJAX utilise le format de fichier .resx déjà utilisé par les applications .NET, qui fournit la prise en charge intégrée des IDE et un type de ressource partageable.

Ce livre blanc est basé sur la version bêta 2 de Microsoft Visual Studio 2008. Ce livre blanc suppose également que vous allez travailler avec Visual Studio 2008, pas Visual Web Developer Express et fournissez des procédures pas à pas en fonction de l’interface utilisateur de Visual Studio. Des exemples de code vont utiliser des modèles de projet qui est peut-être pas disponibles dans Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*La nécessité pour la localisation*

En particulier pour les développeurs d’applications entreprise et les développeurs de composants, la possibilité de créer des outils qui peuvent être conscients des différences entre les langues et cultures devient nécessaire. Conception de composants avec la possibilité de s’adapter aux paramètres régionaux du client augmente la productivité des développeurs et réduit la quantité de travail requise pour l’adaptation d’un composant de fonctionner dans le monde entier.

La localisation est le processus de conception et l’intégration de la prise en charge pour une langue spécifique et une culture dans une application ou un composant d’application. La plateforme Microsoft ASP.NET fournit la prise en charge complète pour la localisation pour les applications ASP.NET standards en intégrant le modèle de localisation .NET standard ; l’infrastructure AJAX de Microsoft utilisent le modèle intégré pour prendre en charge les divers scénarios dans lesquels la localisation peut être effectuée. Avec l’infrastructure AJAX de Microsoft, les scripts peuvent être localisés par en cours de déploiement dans des assemblys satellites, ou en utilisant une structure de système de fichiers statiques.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Intégration des Scripts avec assemblys satellites*

Conformément à la stratégie de localisation .NET Framework standard, les ressources peuvent être incluses dans des assemblys satellites. Les assemblys satellites présentent plusieurs avantages sur l’inclusion de ressource traditionnels dans les fichiers binaires - n’importe quel localisation donnée peut être mis à jour sans mettre à jour de l’image plus grande, localisations supplémentaires peuvent être déployées en installant les assemblys satellites en simplement le dossier du projet et les assemblys satellites peuvent être déployés sans provoquant un rechargement de l’assembly de projet principal. En particulier dans les projets ASP.NET, cela est utile, car elle peut réduire considérablement la quantité de ressources système utilisées par les mises à jour incrémentielles et perturbe au minimum l’utilisation de site Web de production.

Des scripts sont intégrés dans des assemblys en les incluant dans gérée .resx (ou compilée .resources) des fichiers qui sont inclus dans l’assembly au moment de la compilation. Leurs ressources sont accessibles à l’application de script dans le code généré par le runtime AJAX, via les attributs de niveau assembly

*Conventions d’affectation de noms pour les fichiers de Script incorporés*

La gestion de script Microsoft AJAX Framework prend en charge une variété d’options de déploiement et de test de scripts et les instructions sont fournies pour faciliter ces options.

*Pour faciliter le débogage :*

Scripts de publication (production) ne doivent pas inclure le `.debug` qualificateur dans le nom de fichier. Scripts conçus pour le débogage doit inclure `.debug` dans le nom de fichier.

*Pour faciliter la localisation :*

Scripts de la culture neutre ne doivent pas inclure n’importe quel identificateur de culture, le nom du fichier. Pour les scripts qui contiennent des ressources localisées, le code de langue ISO doit être spécifié dans le nom de fichier. Par exemple, `es-CO` signifie pour l’espagnol, Columbia.

Le tableau suivant résume le conventions avec des exemples de nom de fichier :

| Nom de fichier | Signification |
| --- | --- |
| Script.js | Un script de culture neutre-version release. |
| Script.debug.js | Un script indépendant de la culture de la version de débogage. |
| Script.en-US.js | Un script des États-Unis de l’anglais, de version de mise en production. |
| Script.debug.es-CO.js | Un script de Columbia espagnol, version de débogage. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Procédure pas à pas : Créer un Script incorporé, localisé

*Remarque : cette procédure pas à pas requiert l’utilisation de Visual Studio 2008 que Visual Web Developer Express n’inclut pas un modèle de projet pour les projets de bibliothèque de classes.*

1. Créer un nouveau projet de Site Web avec les Extensions ASP.NET AJAX intégré. Créez un autre projet, un projet de bibliothèque de classes, au sein de la solution appelée LocalizingResources.
2. Ajoutez un fichier Jscript nommé VerifyDeletion.js au projet LocalizingResources, ainsi que les fichiers de ressources .resx appelés DeletionResources.resx et DeletionResources.es.resx. Le premier contiendra les ressources de culture neutre ; ce dernier contient des ressources espagnol.
3. Ajoutez le code suivant à VerifyDeletion.js :

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Pour ceux êtes pas familiarisé avec la syntaxe JavaScript Regex, texte dans les barres obliques uniques (dans l’exemple précédent, /FILENAME/ est un exemple) indique un objet RegExp. MSDN Library contient une référence complète de JavaScript et ressources sur des objets JavaScript natifs sont accessibles en ligne.

1. Ajoutez les chaînes de ressources suivant à DeletionResources.resx : 

    **VerifyDelete**: Êtes-vous sûr de que vouloir supprimer le nom de fichier ?

    **Supprimé**: Nom de fichier a été supprimé.

1. Ajoutez les chaînes de ressources suivant à DeletionResources.es.resx : 

    **VerifyDelete**: Outil est seguro que desee quitar nom de fichier ?

    **Supprimé**: Nom de fichier se ha quitado.
2. Ajoutez les lignes de code suivantes au fichier AssemblyInfo :

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Ajouter des références à System.Web et System.Web.Extensions au projet LocalizingResources.
2. Ajoutez une référence au projet LocalizingResources à partir du projet de Site Web.
3. Dans default.aspx, sous le projet de Site Web, vous devez mettre à jour le contrôle ScriptManager avec le balisage supplémentaire suivant :

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Dans default.aspx, n’importe où dans la page, ajoutez ce balisage :

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Appuyez sur F5. Si vous y êtes invité, activez le débogage. Lorsque la page est chargée, appuyez sur le bouton Supprimer. Notez que vous êtes invité en anglais (à moins que votre ordinateur est configuré à préférer les ressources espagnol par défaut) à confirmer l’opération.
2. Fermez la fenêtre du navigateur et retournez à default.aspx. Dans la @Page directive d’en-tête, auto de remplacement pour la Culture et UICulture avec es-ES. Appuyez sur F5 pour lancer l’application web dans le navigateur à nouveau. Cette fois-ci, notez que vous êtes invité à supprimer le fichier en espagnol :


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-localization/_static/image6.png))


Notez qu’il existe plusieurs variantes pour cette procédure pas à pas. Par exemple, les scripts pu être enregistrés avec le contrôle ScriptManager par programme au cours du chargement de page.

## <a name="including-a-static-script-file-structure"></a>*Y compris une Structure de fichier de Script statique*

Lorsque vous utilisez des fichiers de script statiques pour le déploiement, vous perdez certains des avantages d’utiliser le jeu de localisation .NET inhérent. Est principalement visible que vous perdez le type automatique généré à partir, y compris les fichiers de ressources de script ; dans la procédure ci-dessus, par exemple, les ressources ont été exposées par un type généré automatiquement appelé Message à partir du contrôle ScriptManager.

Toutefois, il existe des avantages à l’aide d’une structure de fichier de script statique. Mises à jour peuvent être effectuées sans recompiler et redéployer les assemblys satellites, et l’utilisation d’une structure de fichiers statiques permettre également être effectuée pour remplacer le script incorporé, pour intégrer une partie mineure des fonctionnalités qui n’ont ne peut-être pas été expédiées avec un composant.

Microsoft vous recommande d’éviter un problème de contrôle de version en générant automatiquement vos ressources de script pendant la compilation du projet. Lors de la mise à jour un code de script complète base, il peut devenir plus en plus difficile de garantir que les modifications de code sont répercutées dans chaque script localisés. Comme alternative, vous pouvez simplement maintenir un script de logique et de plusieurs scripts de localisation, fusion des fichiers lors de la génération du projet.

Étant donné que les ressources sont à inclure de manière déclarative, script statique, les fichiers doivent être référencées en ajoutant `<asp:ScriptElement>` éléments en tant qu’enfant de le `<Scripts>` balise du contrôle ScriptManager, ou en ajoutant par programme `ScriptReference` objets pour le `Scripts` propriété de la `ScriptManager` contrôle sur la page lors de l’exécution.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Le ScriptManager et son rôle dans la localisation*

Le ScriptManager active plusieurs comportements automatiques pour les applications localisées :

- Il localise automatiquement les fichiers de script basés sur les paramètres et les conventions d’affectation de noms ; par exemple, il charge les scripts de débogage activé en mode débogage, et charges localisées des scripts basés sur la sélection de l’interface utilisateur du navigateur.
- Il permet la définition des cultures, y compris les cultures personnalisées.
- Il permet la compression des fichiers de script via HTTP.
- Il met en cache des scripts pour gérer efficacement le nombre de demandes.
- Il ajoute une couche d’indirection vers des scripts en les transférant via une URL chiffrée.

Références de script peuvent être ajoutés au contrôle ScriptManager par programmation ou par le balisage déclaratif. Balisage déclaratif est particulièrement utile lorsque vous travaillez avec des scripts incorporés dans des assemblys autres que le projet de site web lui-même, comme le nom du script ne sera probablement pas modifiée que révisions sont envoyées via.

## <a name="summary"></a>Récapitulatif

À mesure que les applications web se développent pour atteindre une audience plus large, le besoin d’être en mesure d’atteindre plus large des cultures et des Communautés devienne core à un modèle d’entreprise ; applications web de commerce électronique doivent être en mesure de gérer avec des devises étrangères, les systèmes de gestion de contenu doivent être présents en mesure non seulement leur contenu, mais également les indicateurs de navigation et les champs de formulaire dans d’autres langages et les entreprises doivent savoir que ce besoin accessible.

Le .NET Framework intrinsèquement prend en charge une infrastructure riche de localisation, les assemblys satellites et fichiers de ressources (.resx) XML pour présenter une méthode uniforme pour rechercher des images et des chaînes de ressources. Les Extensions ASP.NET AJAX, y compris l’infrastructure AJAX de Microsoft et le Script Microsoft AJAX Library, prennent en charge ce modèle de programmation dans le code côté client, l’activation des recherches de chaîne de ressource facile. Les assemblys satellites prennent en charge l’inscription automatique des ressources de script (fichiers .js réelle) via ScriptResource.axd tant que les noms de fichiers suivent un schéma d’affectation de noms donné. Avec cette prise en charge, les Extensions ASP.NET AJAX simplifient la localisation des scripts et la globalisation des applications.

## <a name="bio"></a>*Bio*

Scott Cate travaille avec les technologies Web Microsoft depuis 1997 et est le président de myKB.com ([www.myKB.com](http://www.myKB.com)) où il est spécialisé dans l’écriture d’ASP.NET en fonction des applications axées sur les solutions logicielles de la Base de connaissances. Scott peut être contacté par courrier électronique en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Suivant](understanding-asp-net-ajax-web-services.md)
