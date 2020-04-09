---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'ASP.NET déploiement Web à l’aide de Visual Studio: Web.config File Transformations (fr) Microsoft Docs'
author: tdykstra
description: Cette série de tutoriels vous montre comment déployer (publier) une application web ASP.NET à Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, par usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676124"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>ASP.NET déploiement Web à l’aide de Visual Studio: Web.config File Transformations

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger Starter Project](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de tutoriels vous montre comment déployer (publier) une application web ASP.NET à Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, en utilisant Visual Studio 2012 ou Visual Studio 2010. Pour plus d’informations sur la série, voir [le premier tutoriel de la série](introduction.md).

## <a name="overview"></a>Vue d'ensemble

Ce tutoriel vous montre comment automatiser le processus de modification du fichier *Web.config* lorsque vous le déployez dans différents environnements de destination. La plupart des applications ont des paramètres dans le fichier *Web.config* qui doivent être différents lorsque l’application est déployée. L’automatisation du processus d’apport de ces modifications vous empêche d’avoir à les faire manuellement chaque fois que vous vous déployez, ce qui serait fastidieux et sujet aux erreurs.

Rappel: Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas que vous passez par le tutoriel, assurez-vous de vérifier la [page de dépannage](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformations Web.config par rapport aux paramètres du déploiement Web

Il existe deux façons d’automatiser le processus de modification des paramètres de fichiers *Web.config* : les [transformations Web.config](https://msdn.microsoft.com/library/dd465326.aspx) et [les paramètres de déploiement Web.](https://msdn.microsoft.com/library/ff398068.aspx) Un fichier de transformation *Web.config* contient une balisage XML qui spécifie comment modifier le fichier *Web.config* lorsqu’il est déployé. Vous pouvez spécifier différentes modifications pour des configurations de construction spécifiques et pour des profils de publication spécifiques. Les configurations de construction par défaut sont Debug et Release, et vous pouvez créer des configurations de construction personnalisées. Un profil de publication correspond généralement à un environnement de destination. (Vous en apprendrez davantage sur les profils de publication dans le [déploiement de l’IIS en tant que](deploying-to-iis.md) tutoriel Test Environment.)

Les paramètres de déploiement Web peuvent être utilisés pour spécifier de nombreux types de paramètres qui doivent être configurés pendant le déploiement, y compris les paramètres qui se trouvent dans les fichiers *Web.config.* Lorsqu’il est utilisé pour spécifier les modifications de fichiers *Web.config,* les paramètres de déploiement Web sont plus complexes à configurer, mais ils sont utiles lorsque vous ne connaissez pas la valeur à définir jusqu’à votre déploiement. Par exemple, dans un environnement d’entreprise, vous pouvez créer un *package de déploiement* et le donner à une personne du département informatique à installer en production, et cette personne doit être en mesure d’entrer des chaînes de connexion ou des mots de passe que vous ne connaissez pas.

Pour le scénario que cette série de tutoriel couvre, vous savez à l’avance tout ce qui doit être fait pour le fichier *Web.config,* de sorte que vous n’avez pas besoin d’utiliser les paramètres de déploiement Web. Vous configurerez certaines transformations qui diffèrent selon la configuration de construction utilisée, et certaines qui diffèrent selon le profil de publication utilisé.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Spécifier les paramètres Web.config en Azure

Si les paramètres de fichiers *Web.config* que `<connectionStrings>` vous `<appSettings>` souhaitez modifier sont dans l’élément ou l’élément, et si vous déployez sur les applications Web dans Azure App Service, vous avez une autre option pour automatiser les modifications pendant le déploiement. Vous pouvez entrer les paramètres que vous souhaitez prendre effet dans Azure dans **l’onglet Configurer** de la page portail de gestion de votre application web (faites défiler vers le bas vers les **paramètres de l’application** et les **sections de chaîne de connexion).** Lorsque vous déployez le projet, Azure applique automatiquement les modifications. Pour plus d’informations, voir [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Fichiers de transformation par défaut

Dans **Solution Explorer**, étendre *Web.config* pour voir les fichiers de transformation *Web.Debug.config* et *Web.Release.config* qui sont créés par défaut pour les deux configurations de construction par défaut.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Vous pouvez créer des fichiers de transformation pour les configurations de construction personnalisées en cliquant à droite sur le fichier Web.config et en choisissant **Add Config Transforms** à partir du menu context. Pour ce tutoriel, vous n’avez pas besoin de le faire, et l’option de menu est désactivé, parce que vous n’avez pas créé de configurations de construction personnalisées.

Plus tard, vous créerez trois autres fichiers de transformation, un chacun pour le test, la mise en scène et la production publier des profils. Un exemple typique d’un paramètre que vous géreriez dans un fichier de transformation de profil de publication parce qu’il dépend de l’environnement de destination est un critère d’évaluation WCF qui est différent pour le test par rapport à la production. Vous allez créer des fichiers de transformation de profil dans des tutoriels ultérieurs après avoir créé les profils de publication qu’ils vont avec.

## <a name="disable-debug-mode"></a>Mode de débogé de débinging invalide

Un exemple d’un paramètre qui dépend de `debug` la configuration de construction plutôt que de l’environnement de destination est l’attribut. Pour une version de version, vous voulez généralement débogage désactivé indépendamment de l’environnement que vous déployez. Par conséquent, par défaut, les modèles de projet Visual Studio créent des `debug` fichiers `compilation` de transformation *Web.Release.config* avec du code qui supprime l’attribut de l’élément. Voici le par défaut *Web.Release.config*: en plus d’un code de `compilation` transformation de `debug` l’échantillon qui est commenté, il inclut le code dans l’élément qui supprime l’attribut :

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

L’attribut `xdt:Transform="RemoveAttributes(debug)"` précise que vous `debug` souhaitez que l’attribut soit supprimé de l’élément `system.web/compilation` du fichier *Web.config* déployé. Cela se fera chaque fois que vous déployez une version de version.

## <a name="limit-error-log-access-to-administrators"></a>Limiter l’accès au journal d’erreur aux administrateurs

S’il y a une erreur pendant l’exécution de l’application, l’application affiche une page d’erreur générique à la place de la page d’erreur générée par le système, et elle utilise le [paquet Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) pour l’enregistrement d’erreurs et les rapports. L’élément `customErrors` du fichier *Web.config* de l’application spécifie la page d’erreur :

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Pour voir la page d’erreur, modifiez temporairement l’attribut `mode` de l’élément `customErrors` de "RemoteOnly" à "On" et exécutez l’application à partir de Visual Studio. Provoquer une erreur en demandant une URL invalide, comme *Studentsxxx.aspx*. Au lieu d’une page d’erreur générée par l’IIS « La ressource ne peut pas être trouvée », vous voyez la page *GenericErrorPage.aspx.*

![Page d’erreur](web-config-transformations/_static/image2.png)

Pour voir le journal d’erreur, remplacez tout dans l’URL après `http://localhost:51130/elmah.axd`le numéro de port par *elmah.axd* (par exemple, ) et appuyez sur Entrez :

![Page ELMAH](web-config-transformations/_static/image3.png)

N’oubliez pas de `customErrors` remettre l’élément en mode "RemoteOnly" lorsque vous avez terminé.

Sur votre ordinateur de développement, il est pratique de permettre un accès gratuit à la page de journal d’erreur, mais dans la production qui serait un risque pour la sécurité. Pour le site de production, vous souhaitez ajouter une règle d’autorisation qui restreint l’accès au journal d’erreur aux administrateurs, et pour vous assurer que la restriction fonctionne que vous le souhaitez également dans le test et la mise en scène. C’est donc un autre changement que vous souhaitez implémenter chaque fois que vous déployez une version de version, et il appartient donc dans le fichier *Web.Release.config.*

Ouvrez *Web.Release.config* et `location` ajoutez un `configuration` nouvel élément immédiatement avant l’étiquette de clôture, comme indiqué ici.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

La `Transform` valeur d’attribut de `location` "Insert" fait ajouter cet `location` élément en tant que frère ou sœur à tous les éléments existants dans le fichier *Web.config.* (Il y `location` a déjà un élément qui spécifie les règles d’autorisation pour la page **des crédits de mise à jour.)**

Maintenant, vous pouvez prévisualiser la transformation pour vous assurer que vous l’avez codé correctement.

Dans **Solution Explorer**, cliquez à droite sur *Web.Release.config* et cliquez sur Preview **Transform**.

![Aperçu Du menu Transform](web-config-transformations/_static/image4.png)

Une page s’ouvre qui vous montre le fichier *Web.config* de développement sur la gauche et ce que le fichier *Web.config* déployé ressemblera sur la droite, avec des changements mis en évidence.

![Aperçu de la transformation de débog](web-config-transformations/_static/image5.png)

![Aperçu de la transformation de l’emplacement](web-config-transformations/_static/image6.png)

( Dans l’aperçu, vous remarquerez peut-être quelques modifications supplémentaires pour lesquelles vous n’avez pas écrit des transformations : celles-ci impliquent généralement la suppression de l’espace blanc qui n’affecte pas la fonctionnalité.)

Lorsque vous testez le site après le déploiement, vous testez également pour vérifier que la règle d’autorisation est efficace.

> [!NOTE] 
> 
> **Note de sécurité** N’affichez jamais de détails d’erreur au public dans une demande de production, ou stockez ces renseignements dans un lieu public. Les attaquants peuvent utiliser des informations d’erreur pour découvrir les vulnérabilités d’un site. Si vous utilisez ELMAH dans votre propre application, configurez ELMAH pour minimiser les risques de sécurité. L’exemple ELMAH dans ce tutoriel ne doit pas être considéré comme une configuration recommandée. C’est un exemple qui a été choisi afin d’illustrer comment gérer un dossier dans lequel l’application doit être en mesure de créer des fichiers. Pour plus d’informations, voir [sécurisation du critère d’évaluation ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un paramètre que vous gérerez dans les fichiers de transformation de profils

Un scénario commun est d’avoir des paramètres de fichiers *Web.config* qui doivent être différents dans chaque environnement que vous déployez. Par exemple, une application qui appelle un service WCF peut avoir besoin d’un point de terminaison différent dans les environnements de test et de production. L’application de l’Université Contoso comprend également un cadre de ce type. Ce paramètre contrôle un indicateur visible sur les pages d’un site qui vous indique dans quel environnement vous vous trouvez, comme le développement, le test ou la production. La valeur de réglage détermine si l’application va ajouter "(Dev)" ou "(Test)" au titre principal de la page principale *du master Site.Master:*

![Indicateur de l’environnement](web-config-transformations/_static/image7.png)

L’indicateur de l’environnement est omis lorsque l’application est en cours d’exécution dans la mise en scène ou la production.

Les pages Web de l’Université Contoso lisent une valeur qui est établie dans `appSettings` le fichier *Web.config* afin de déterminer dans quel environnement l’application est en cours d’exécution :

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

La valeur doit être "Test" dans l’environnement d’essai, et "Prod" pour la mise en scène et la production.

Le code suivant dans un fichier de transformation mettra en œuvre cette transformation :

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

La `xdt:Transform` valeur d’attribut "SetAttributes" indique que le but de cette transformation est de changer les valeurs d’attribut d’un élément existant dans le fichier *Web.config.* La `xdt:Locator` valeur d’attribut "Match(key)" indique que l’élément à modifier est celui dont `key` l’attribut correspond à l’attribut `key` spécifié ici. Le seul autre `add` attribut `value`de l’élément est, et c’est ce qui sera changé dans le fichier *Web.config* déployé. Le code montré `value` ici provoque `Environment` `appSettings` l’attribut de l’élément à définir à "Test" dans le fichier *Web.config* qui est déployé.

Cette transformation appartient aux fichiers de transformation du profil de publication, que vous n’avez pas encore créés. Vous créerez et mettrez à jour les fichiers de transformation qui implémentent cette modification lorsque vous créez les profils de publication pour les environnements de test, de mise en scène et de production. Vous le ferez dans le [déploiement à l’IIS](deploying-to-iis.md) et [déployez-vous dans des](deploying-to-production.md) tutoriels de production.

> [!NOTE]
> Parce que ce `<appSettings>` paramètre est dans l’élément, vous avez une autre alternative pour spécifier la transformation lorsque vous déployez vers des applications Web dans Azure App Service Voir [spécifier les paramètres Web.config dans Azure](#watransforms) plus tôt dans ce sujet.

## <a name="setting-connection-strings"></a>Définition des chaînes de connexion

Bien que le fichier de transformation par défaut contient un exemple qui montre comment mettre à jour une chaîne de connexion, dans la plupart des cas, vous n’avez pas besoin de configurer des transformations de chaîne de connexion, car vous pouvez spécifier les chaînes de connexion dans le profil de publication. Vous le ferez dans le [déploiement à l’IIS](deploying-to-iis.md) et [déployez-vous dans des](deploying-to-production.md) tutoriels de production.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant fait autant que vous pouvez avec les transformations *Web.config* avant de créer les profils de publication, et vous avez vu un aperçu de ce qui sera dans le fichier Web.config déployé.

![Aperçu de la transformation de l’emplacement](web-config-transformations/_static/image8.png)

Dans le tutoriel suivant, vous vous occuperez des tâches de configuration de déploiement qui nécessitent le réglage des propriétés du projet.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur les sujets abordés par ce tutoriel, voir [l’utilisation des transformations Web.config pour changer les paramètres dans le fichier Web.config de destination ou le fichier app.config lors](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) du déploiement dans la carte de contenu de déploiement Web pour Visual Studio et ASP.NET.

> [!div class="step-by-step"]
> [Suivant précédent](preparing-databases.md)
> [Next](project-properties.md)
