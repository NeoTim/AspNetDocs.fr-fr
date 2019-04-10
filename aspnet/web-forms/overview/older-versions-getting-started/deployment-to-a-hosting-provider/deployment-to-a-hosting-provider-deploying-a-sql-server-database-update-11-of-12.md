---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : Déploiement d’une mise à jour de base de données de serveur SQL - 11 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 8f2c0e2ea50bccfd300e522dcf8b2ed5ab67cf83
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59408132"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : Déploiement d’une mise à jour de base de données de serveur SQL - 11 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour une introduction à la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer sur les Sites Web Windows Azure, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel vous montre comment déployer une mise à jour de la base de données dans une base de données SQL Server complète. Étant donné que les Migrations Code First fait tout le travail de mise à jour de la base de données, le processus est presque identique à ce que vous l’avez fait pour SQL Server Compact dans le [déploiement d’une mise à jour de la base de données](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) didacticiel.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Ajout d’une nouvelle colonne à une Table

Dans cette section du didacticiel, vous apporterez une modification de base de données et les modifications de code correspondant, puis de les tester dans Visual Studio en vue de les déployer sur les environnements de test et de production. La modification implique l’ajout d’un `OfficeHours` colonne à la `Instructor` entité et en affichant les nouvelles informations dans le **formateurs** page web.

Dans le projet ContosoUniversity.DAL, ouvrez *Instructor.cs* et ajoutez la propriété suivante entre le `HireDate` et `Courses` propriétés :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Mettre à jour la classe d’initialiseur afin qu’il alimente la nouvelle colonne de données de test. Ouvrez *migrations\configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` avec le bloc de code suivant, qui inclut la nouvelle colonne :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Dans le projet ContosoUniversity, ouvrez *Instructors.aspx* et ajoutez un nouveau champ de modèle pour les heures de bureau juste avant la fermeture `</Columns>` balise dans la première `GridView` contrôle :

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Générez la solution.

Ouvrez le **Console du Gestionnaire de Package** fenêtre et sélectionnez ContosoUniversity.DAL comme le **projet par défaut**.

Entrez les commandes suivantes :

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Exécutez l’application et sélectionnez le **formateurs** page. La page prend un peu Pluse de temps de chargement, car Entity Framework permet de recréer la base de données et l’alimente avec des données de test.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Déploiement de la mise à jour de la base de données dans l’environnement de Test

Lorsque vous utilisez des Migrations Code First, la méthode pour le déploiement d’une modification de la base de données vers SQL Server est le même que pour SQL Server Compact. Toutefois, vous devez modifier le Test de profil de publication, car elle est toujours configurée pour migrer à partir de SQL Server Compact vers SQL Server.

La première étape consiste à supprimer les transformations de chaîne de connexion que vous avez créé dans le didacticiel précédent. Ils ne sont plus nécessaires, car vous allez spécifier des transformations de chaîne de connexion dans le profil de publication, comme vous le faisiez avant que vous avez configuré le **Package/Publication SQL** onglet pour la migration vers SQL Server.

Ouvrez le *Web.Test.config* de fichiers et de supprimer le `connectionStrings` élément. La seule transformation restante dans le *Web.Test.config* fichier concerne le `Environment` valeur dans le `appSettings` élément.

Maintenant, vous pouvez modifier le profil de publication et le publier à l’environnement de test.

Ouvrez le **publier le site Web** Assistant, puis cliquez sur le **profil** onglet.

Sélectionnez le **Test** le profil de publication.

Sélectionnez l’onglet **Paramètres**.

Cliquez sur **activer la base de données de publication améliorations**.

Dans la zone chaîne de connexion pour **SchoolContext**, entrez la même valeur que vous avez utilisé dans le *Web.Test.config* fichier de transformation dans le didacticiel précédent :

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Sélectionnez **exécuter les Migrations Code First (s’exécute sur le démarrage de l’application)**. (Dans votre version de Visual Studio, la case à cocher doit être intitulé **appliquer des Migrations Code First**.)

Dans la zone chaîne de connexion pour **DefaultConnection**, entrez la même valeur que vous avez utilisé dans le *Web.Test.config* fichier de transformation dans le didacticiel précédent :

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Laissez **base de données de mise à jour** désactivée.

Cliquez sur **Publier**.

Visual Studio déploie les modifications de code dans l’environnement de test et ouvre le navigateur à la page d’accueil de Contoso University.

Sélectionnez la page des formateurs.

Lorsque l’application s’exécute à cette page, il essaie d’accéder à la base de données. Migrations Code First vérifie si la base de données est en cours et qu’il détecte que la dernière migration n’a pas encore été appliquée. Migrations Code First s’applique la dernière migration, s’exécute le `Seed` (méthode), puis sur la page s’exécute normalement. Vous consultez la nouvelle colonne des heures de bureau avec les données de départ.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Déploiement de la mise à jour de la base de données dans l’environnement de Production

Vous devez modifier le profil de publication pour l’environnement de production également. Dans ce cas, vous allez supprimer le profil existant et créez-en un en important un fichier .publishsettings mis à jour. Le fichier mis à jour inclura la chaîne de connexion pour la base de données SQL Server Cytanium.

Comme vous l’avez vu lorsque vous avez déployé à l’environnement de test, vous n’avez plus besoin transformations de chaîne de connexion dans le *Web.Production.config* fichier de transformation. Ouvrez ce fichier, supprimez le `connectionStrings` élément. Les transformations restantes sont pour le `Environment` valeur dans le `appSettings` élément et le `location` élément qui limite l’accès aux rapports d’erreurs Elmah.

Avant de créer un nouveau profil de publication pour la production, télécharger un fichier .publishsettings mis à jour la même façon que vous l’avez fait précédemment dans le [déploiement dans l’environnement de Production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) didacticiel. (Dans le panneau de configuration de Cytanium, cliquez sur **Sites Web**, puis cliquez sur le **contosouniversity.com** site Web. Sélectionnez le **publication Web** onglet, puis cliquez sur **télécharger le profil de publication pour ce site web**.) La raison pour laquelle que vous effectuez cette opération consiste à choisir la chaîne de connexion de base de données dans le fichier .publishsettings. La chaîne de connexion n’était pas disponible la première fois que vous avez téléchargé le fichier, car vous étiez toujours à l’aide de SQL Server Compact et que vous n’aviez pas la base de données SQL Server à Cytanium encore créé.

Maintenant, vous pouvez modifier le profil de publication et le publier à l’environnement de production.

Ouvrez le **publier le site Web** Assistant, puis cliquez sur le **profil** onglet.

Cliquez sur **gérer les profils**, puis supprimez le profil de Production.

Fermer le **publier le site Web** Assistant pour enregistrer cette modification.

Ouvrez le **publier le site Web** Assistant à nouveau, puis cliquez sur **importation**.

Sur le **connexion** , modifiez **URL de Destination** à la valeur appropriée si vous utilisez une URL temporaire.

Cliquez sur **Suivant**.

Sur le **paramètres** , cliquez sur **activer la base de données de publication améliorations**.

Dans la liste de liste déroulante de chaîne de connexion pour **SchoolContext**, sélectionnez la chaîne de connexion Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Sélectionnez **Execute Code First migrations (s’exécute sur le démarrage de l’application)**.

Dans la liste de liste déroulante de chaîne de connexion pour **DefaultConnection**, sélectionnez la chaîne de connexion Cytanium.

Sélectionnez le **profil** , cliquez sur **gérer les profils**et renommer le profil « contosouniversity.com - Web Deploy » en « Production ».

Fermez le profil de publication pour enregistrer la modification, puis ouvrez-le à nouveau.

Cliquez sur **Publier**. (Pour un site Web de production réel, vous copiez *application\_offline.htm* de production et put dans votre dossier de projet avant la publication, puis le supprimer lorsque le déploiement est terminé.)

Visual Studio déploie les modifications de code dans l’environnement de test et ouvre le navigateur à la page d’accueil de Contoso University.

Sélectionnez la page des formateurs.

Migrations Code First met à jour la base de données de la même manière que dans l’environnement de Test. Vous consultez la nouvelle colonne des heures de bureau avec les données de départ.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Vous avez déployé une mise à jour d’application qui comportait une modification de la base de données, à l’aide d’une base de données SQL Server.

## <a name="more-information"></a>Informations complémentaires

Cette étape termine cette série de didacticiels sur le déploiement d’une application web ASP.NET sur un fournisseur d’hébergement tiers. Pour plus d’informations sur les sujets couverts dans ces didacticiels, consultez le [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) sur le site web MSDN.

## <a name="acknowledgements"></a>Remerciements

J’aimerais remercier les personnes suivantes qui a apporté d’importantes contributions au contenu de cette série de didacticiels :

- [Alberto Poblacion, MVP &amp; MCT, Espagne](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, MVP de développement de plate-forme de données, États-Unis
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italie](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter : [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter : [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter : [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbie](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter : [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
