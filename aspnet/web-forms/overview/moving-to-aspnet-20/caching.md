---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Caching - France Microsoft Docs
author: rick-anderson
description: Une compréhension de la mise en cache est importante pour une application ASP.NET performante. ASP.NET 1.x offrait trois options différentes pour la mise en cache; mise en cache de sortie,...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: a199a9c0352dfb054e8d4e5e67652db9bd38851c
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81542935"
---
# <a name="caching"></a>Mise en cache

par [Microsoft](https://github.com/microsoft)

> Une compréhension de la mise en cache est importante pour une application ASP.NET performante. ASP.NET 1.x offrait trois options différentes pour la mise en cache; mise en cache, mise en cache de fragments et cache API.

Une compréhension de la mise en cache est importante pour une application ASP.NET performante. ASP.NET 1.x offrait trois options différentes pour la mise en cache; mise en cache, mise en cache de fragments et cache API. ASP.NET 2.0 offre ces trois méthodes, mais il ajoute quelques fonctionnalités supplémentaires significatives. Il existe plusieurs nouvelles dépendances de cache et les développeurs ont maintenant la possibilité de créer des dépendances de cache personnalisée ainsi. La configuration de la mise en cache a également été considérablement améliorée dans ASP.NET 2.0.

## <a name="new-features"></a>Nouvelles fonctionnalités

## <a name="cache-profiles"></a>Profils Cache

Les profils de cache permettent aux développeurs de définir des paramètres de cache spécifiques qui peuvent ensuite être appliqués à des pages individuelles. Par exemple, si vous avez quelques pages qui doivent être expirées du cache après 12 heures, vous pouvez facilement créer un profil de cache qui peut être appliqué à ces pages. Pour ajouter un nouveau profil &lt;de cache, utilisez&gt; la section outputCacheSettings dans le fichier de configuration. Par exemple, ci-dessous est la configuration d’un profil de cache appelé *deux jours* qui configure une durée de cache de 12 heures.

[!code-xml[Main](caching/samples/sample1.xml)]

Pour appliquer ce profil de cache à une page particulière, utilisez l’attribut CacheProfile de la directive OutputCache comme indiqué ci-dessous :

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dépendances de cache personnalisées

ASP.NET développeurs de 1,0 ont crié pour les dépendances de cache personnalisées. Dans ASP.NET 1.x, la classe CacheDependency a été scellée qui a empêché les développeurs de lui enlever leurs propres classes. Dans ASP.NET 2.0, cette limitation est supprimée et les développeurs sont libres de développer leurs propres dépendances de cache personnalisée. La classe CacheDependency permet la création d’une dépendance personnalisée au cache basée sur des fichiers, des répertoires ou des clés de cache.

Par exemple, le code ci-dessous crée une nouvelle dépendance à cache personnalisée basée sur un fichier appelé stuff.xml situé dans la racine de l’application Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

Dans ce scénario, lorsque le fichier stuff.xml change, l’élément mis en cache est invalidé.

Il est également possible de créer une dépendance à cache personnalisée à l’aide de clés de cache. En utilisant cette méthode, la suppression de la clé de cache invalidera les données mises en cache. L'exemple suivant illustre ce mécanisme :

[!code-csharp[Main](caching/samples/sample4.cs)]

Pour invalider l’élément qui a été inséré ci-dessus, retirez simplement l’élément qui a été inséré dans le cache pour agir comme clé de cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Notez que la clé de l’élément qui agit comme la clé de cache doit être la même que la valeur ajoutée à la gamme de clés de cache.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Dépendances à cache SQL axées sur les sondages (aussi appelées dépendances à base de table)

SQL Server 7 et 2000 utilisent le modèle basé sur les sondages pour les dépendances de cache SQL. Le modèle basé sur les sondages utilise un déclencheur sur une table de base de données qui est déclenchée lorsque les données dans le tableau changent. Ce déclencheur met à jour un champ **changeId** dans le tableau de notification qui ASP.NET vérifie périodiquement. Si le champ **changeId** a été mis à jour, ASP.NET sait que les données ont changé et qu’elles invalident les données mises en cache.

> [!NOTE]
> SQL Server 2005 peut également utiliser le modèle basé sur les sondages, mais comme le modèle basé sur les sondages n’est pas le modèle le plus efficace, il est conseillé d’utiliser un modèle basé sur la requête (discuté plus tard) avec SQL Server 2005.

Pour qu’une dépendance à la cache SQL utilisant le modèle basé sur les sondages fonctionne correctement, les tableaux doivent avoir des notifications activées. Ceci peut être accompli programmatiquement en utilisant la classe SqlCacheDependencyAdmin ou en utilisant l’utilitaire aspnet\_regsql.exe.

La ligne de commande suivante enregistre le tableau produits dans la base de données Northwind située sur une instance SQL Server nommée *dbase* pour la dépendance au cache SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Ce qui suit est une explication des commutateurs de ligne de commande utilisés dans la commande ci-dessus :

| **Commutateur de ligne de commande** | **Objectif** |
| --- | --- |
| -S *server* | Spécifie le nom du serveur. |
| -ed | Précise que la base de données doit être activée pour la dépendance au cache SQL. |
| -d *\_nom de base de données* | Spécifie le nom de base de données qui doit être activé pour la dépendance au cache SQL. |
| -E | Spécifie que\_aspnet regsql devrait utiliser l’authentification Windows lors de la connexion à la base de données. |
| -et | Précise que nous permettons une table de base de données pour la dépendance au cache SQL. |
| -t *\_nom de table* | Précise le nom de la table de base de données pour permettre la dépendance au cache SQL. |

> [!NOTE]
> Il ya d’autres commutateurs\_disponibles pour aspnet regsql.exe. Pour une liste complète,\_exécuter aspnet regsql.exe -? d’une ligne de commandement.

Lorsque cette commande s’exécute, les modifications suivantes sont apportées à la base de données SQL Server :

- Une table **AspNet\_SqlCacheTablesForChangeNotification** est ajoutée. Ce tableau contient une rangée pour chaque table de la base de données pour laquelle une dépendance au cache SQL a été activée.
- Les procédures stockées suivantes sont créées à l’intérieur de la base de données :

| AspNet\_SqlCachePollingProcedure | Interroge la table\_AspNet SqlCacheTablesForChangeNotification et renvoie toutes les tables qui sont activées pour la dépendance au cache SQL et la valeur de changeId pour chaque table. Ce proc stocké est utilisé pour le sondage pour déterminer si les données ont changé. |
| --- | --- |
| AspNet\_SqlCacheQueryRéenindablesStoredProcedure | Retourne toutes les tables activées pour la dépendance au\_cache SQL en interrogeant la table AspNet SqlCacheTablesForChangeNotification et renvoie toutes les tables activées pour la dépendance au cache SQL. |
| AspNet\_SqlCacheRegisterTableProcedure | Enregistre une table pour la dépendance au cache SQL en ajoutant l’entrée nécessaire dans le tableau de notification et ajoute le déclencheur. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Désenregistre une table pour la dépendance au cache SQL en supprimant l’entrée dans le tableau de notification et en supprimant la gâchette. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Mise à jour du tableau de notification en incrémentant le changeId pour le tableau changé. ASP.NET utilise cette valeur pour déterminer si les données ont changé. Comme indiqué ci-dessous, ce proc stocké est exécuté par le déclencheur créé lorsque la table est activée. |

- Un déclencheur SQL Server appelé ** _\_nom_\_de\_table\_AspNet SqlCacheNotification Trigger** est créé pour la table. Ce déclencheur exécute l’AspNet\_SqlCacheUpdateChangeIdStoredProcedure lorsqu’un INSERT, UPDATE ou DELETE est effectué sur la table.
- Un rôle SQL Server appelé **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** est ajouté à la base de données.

Le rôle **\_aspnet\_ChangeNotification ReceiveNotificationsOnlyAccess** SQL Server a des\_autorisations EXEC à l’AspNet SqlCachePollingStoredProcedure. Pour que le modèle de sondage fonctionne correctement, vous devez\_ajouter votre\_compte de processus au rôle aspnet ChangeNotificationsOnlyAccess. L’outil\_aspnet regsql.exe ne le fera pas pour vous.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurer les dépendances sqL cache basées sur les sondages

Il y a plusieurs étapes qui sont nécessaires pour configurer les dépendances de cache SQL basées sur les sondages. La première étape consiste à activer la base de données et le tableau comme nous l’avons vu ci-dessus. Une fois cette étape terminée, le reste de la configuration est le suivant :

- Configurer le fichier de configuration ASP.NET.
- Configurer la dépendance SqlCache

### <a name="configuring-the-aspnet-configuration-file"></a>Configurer le fichier de configuration ASP.NET

En plus d’ajouter une chaîne de connexion comme nous &lt;&gt; l’avons &lt;vu dans un&gt; module précédent, vous devez également configurer un élément de cache avec un élément de dépendance sqlCache comme indiqué ci-dessous :

[!code-xml[Main](caching/samples/sample7.xml)]

Cette configuration permet une dépendance au cache SQL vis-à-vis de la base de données *pubs.* Notez que l’attribut &lt;pollTime dans l’élément sqlCacheDependency&gt; par défaut à 60000 millisecondes ou 1 minute. (Cette valeur ne peut pas être inférieure à 500 millisecondes.) Dans cet exemple,&gt; l’élément d’ajout &lt;ajoute une nouvelle base de données et l’emporte sur le pollTime, le fixant à 900000 millisecondes.

#### <a name="configuring-the-sqlcachedependency"></a>Configurer la dépendance SqlCache

L’étape suivante consiste à configurer le SqlCacheDependency. La façon la plus simple d’y parvenir est de spécifier la valeur de l’attribut SqlDependency dans la directive Outcache comme suit :

[!code-aspx[Main](caching/samples/sample8.aspx)]

Dans la directive ci-dessus - OutputCache, une dépendance de cache SQL est configurée pour la table *des auteurs* dans la base de données *pubs.* Les dépendances multiples peuvent être configurées en les séparant d’un semi-colon comme tel :

[!code-aspx[Main](caching/samples/sample9.aspx)]

Une autre méthode de configuration de la SqlCacheDependency est de le faire programmatiquement. Le code suivant crée une nouvelle dépendance À la cache SQL à l’égard de la table des *auteurs* dans la base de données *des pubs.*

[!code-csharp[Main](caching/samples/sample10.cs)]

L’un des avantages de la définition programmatique de la dépendance au cache SQL est que vous pouvez gérer toutes les exceptions qui pourraient se produire. Par exemple, si vous tentez de définir une dépendance au cache SQL pour une base de données qui n’a pas été activée pour notification, une exception **DatabaseNotEnabledForNotificationException** sera lancée. Dans ce cas, vous pouvez tenter d’activer la base de données pour les notifications en appelant la méthode **SqlCacheDependencyAdmin.EnableNotifications** et en lui transmettant le nom de base de données.

De même, si vous tentez de définir une dépendance à la cache SQL pour une table qui n’a pas été activée pour notification, une **tableNotEnabledForNotificationException** sera lancée. Vous pouvez ensuite appeler la méthode **SqlCacheDependencyAdmin.EnableTableForNotifications** en lui transmettant le nom de base de données et le nom de table.

L’échantillon de code suivant illustre comment configurer correctement la manipulation des exceptions lors de la configuration d’une dépendance au cache SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Plus d’informations:[https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dépendances SQL Cache à base de requêtes (SQL Server 2005 seulement)

Lors de l’utilisation de SQL Server 2005 pour la dépendance au cache SQL, le modèle basé sur les sondages n’est pas nécessaire. Lorsqu’elles sont utilisées avec SQL Server 2005, les dépendances de cache SQL communiquent directement via les connexions SQL à l’instance SQL Server (aucune autre configuration n’est nécessaire) à l’aide de notifications de requête SQL Server 2005.

La façon la plus simple d’activer la notification basée sur les requêtes est de le faire de façon déclarative en définissant **l’attribut SqlCacheDependency** de l’objet source de données à **CommandNotification** et en définissant l’attribut **EnableCaching** à **vrai**. En utilisant cette méthode, aucun code n’est requis. Si le résultat d’une commande exécutée contre la source de données change, il invalidera les données de cache.

L’exemple suivant configure un contrôle de source de données pour la dépendance au cache SQL :

[!code-aspx[Main](caching/samples/sample12.aspx)]

Dans ce cas, si la requête spécifiée dans le **SelectCommand** renvoie un résultat différent de celui qu’elle a obtenu à l’origine, les résultats qui sont mis en cache sont invalidés.

Vous pouvez également spécifier que toutes vos sources de données sont activées pour les dépendances de cache SQL en définissant **l’attribut SqlDependency** de la directive **OutputCache** à **CommandNotification**. L’exemple ci-dessous l’illustre.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Pour plus d’informations sur les notifications de requête dans SQL Server 2005, consultez le SQL Server Books Online.

Une autre méthode de configuration d’une dépendance à la cache SQL basée sur la requête est de le faire programmatiquement en utilisant la classe SqlCacheDependency. L’échantillon de code suivant illustre comment cela est accompli.

[!code-csharp[Main](caching/samples/sample14.cs)]

Plus d’informations:[https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Remplacement post-Cache

Le cachement d’une page peut augmenter considérablement les performances d’une application Web. Cependant, dans certains cas, vous avez besoin de la plupart de la page pour être mis en cache et certains fragments dans la page pour être dynamique. Par exemple, si vous créez une page d’actualités entièrement statique pour les périodes de temps définies, vous pouvez définir la page entière à mettre en cache. Si vous vouliez inclure une bannière publicitaire tournante qui a changé sur chaque demande de page, alors la partie de la page contenant la publicité doit être dynamique. Pour vous permettre de mettre en cache une page mais de remplacer un contenu dynamiquement, vous pouvez utiliser ASP.NET substitution post-cache. Avec la substitution post-cache, la page entière est mise en cache avec des pièces spécifiques marquées comme exemptes de mise en cache. Dans l’exemple des bannières publicitaires, le contrôle AdRotator vous permet de profiter de la substitution post-cache afin que les annonces créées dynamiquement pour chaque utilisateur et pour chaque page actualisent.

Il existe trois façons de mettre en œuvre la substitution post-cache :

- Déclarativement, en utilisant le contrôle de substitution.
- Programmatiquement, en utilisant l’API de contrôle de substitution.
- Implicitement, en utilisant le contrôle AdRotator.

### <a name="substitution-control"></a>Contrôle de substitution

Le contrôle de substitution ASP.NET spécifie une section d’une page mise en cache qui est créée dynamiquement plutôt que mise en cache. Vous placez un contrôle de substitution à l’emplacement sur la page où vous souhaitez que le contenu dynamique s’affiche. Au moment de l’exécution, le contrôle de substitution appelle une méthode que vous spécifiez avec la propriété MethodName. La méthode doit retourner une chaîne, qui remplace ensuite le contenu du contrôle de substitution. La méthode doit être une méthode statique sur le contrôle contenant de la page ou de l’utilisateur. L’utilisation du contrôle de substitution entraîne la possibilité de modifier la cacheabilité du client en cachet du serveur, de sorte que la page ne sera pas mise en cache sur le client. Cela garantit que les futures demandes à la page appellent à nouveau la méthode pour générer du contenu dynamique.

### <a name="substitution-api"></a>Remplacement API

Pour créer du contenu dynamique pour une page mise en cache de manière programmatique, vous pouvez appeler la méthode [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) dans votre code de page, en lui transmettant le nom d’une méthode comme paramètre. La méthode qui gère la création du contenu dynamique prend un seul paramètre [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) et renvoie une chaîne. La chaîne de retour est le contenu qui sera remplacé à l’emplacement donné. Un avantage d’appeler la méthode WriteSubstitution au lieu d’utiliser le contrôle de substitution de façon déclarative est que vous pouvez appeler une méthode de tout objet arbitraire plutôt que d’appeler une méthode statique de la Page ou de l’objet UserControl.

L’appel de la méthode WriteSubstitution permet de modifier la cacheabilité côté client en cachet du serveur, de sorte que la page ne sera pas mise en cache sur le client. Cela garantit que les futures demandes à la page appellent à nouveau la méthode pour générer du contenu dynamique.

### <a name="adrotator-control"></a>Contrôle des adRotator

Le contrôle serveur AdRotator implémente le support pour la substitution post-cache en interne. Si vous placez un contrôle AdRotator sur votre page, il rendra des publicités uniques sur chaque demande, indépendamment du fait que la page mère soit mise en cache. Par conséquent, une page qui comprend un contrôle AdRotator n’est mise en cache que côté serveur.

## <a name="controlcachepolicy-class"></a>Classe ControlCachePolicy

La classe ControlCachePolicy permet le contrôle programmatique de la mise en cache des fragments à l’aide de contrôles utilisateur. ASP.NET intègre les contrôles utilisateur dans une instance [BasePartialCachingControl.](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) La classe BasePartialCachingControl représente un contrôle utilisateur qui a la mise en cache de sortie activée.

Lorsque vous accédez à la propriété [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) d’un contrôle [PartialCachingControl,](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) vous recevrez toujours un objet controlCachePolicy valide. Toutefois, si vous accédez à la propriété [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) [d’un contrôle UserControl,](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) vous ne recevez un objet ControlCachePolicy valide que si le contrôle utilisateur est déjà enveloppé par un contrôle BasePartialCachingControl. S’il n’est pas enveloppé, l’objet ControlCachePolicy retourné par la propriété jettera des exceptions lorsque vous tentez de le manipuler parce qu’il n’a pas un BasePartialCachingControl associé. Pour déterminer si une instance UserControl prend en charge la mise en cache sans générer d’exceptions, inspectez la propriété [SupportsCaching.](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx)

L’utilisation de la classe ControlCachePolicy est l’une des nombreuses façons dont vous pouvez activer la mise en cache de sortie. La liste suivante décrit les méthodes que vous pouvez utiliser pour activer la mise en cache de sortie :

- Utilisez la directive [OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) pour permettre la mise en cache des sorties dans des scénarios déclaratifs.
- Utilisez l’attribut [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) pour activer la mise en cache d’un contrôle utilisateur dans un fichier de code.
- Utilisez la classe ControlCachePolicy pour spécifier les paramètres de cache dans des scénarios programmatiques dans lesquels vous travaillez avec des instances BasePartialCachingControl qui ont été compatibles avec le cache à l’aide d’une des méthodes précédentes et chargées dynamiquement à l’aide de la méthode [System.Web.UI.TemplateControl.LoadControl.](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx)

Une instance ControlCachePolicy ne peut être manipulée avec succès qu’entre les étapes Init et PreRender du cycle de vie de contrôle. Si vous modifiez un objet ControlCachePolicy après la phase PreRender, ASP.NET lance une exception car toute modification apportée après la mise en service du contrôle ne peut pas affecter les paramètres de cache (un contrôle est mis en cache pendant l’étape Du Render). Enfin, une instance de contrôle utilisateur (et donc son objet ControlCachePolicy) n’est disponible que pour la manipulation programmatique lorsqu’il est effectivement rendu.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Modifications apportées à &lt;la&gt; configuration de la mise en cache - L’élément de mise en cache

Il y a plusieurs modifications à la configuration de mise en cache dans ASP.NET 2.0. &lt;L’élément&gt; de mise en cache est nouveau dans ASP.NET 2.0 et vous permet d’effectuer des modifications de configuration de mise en cache dans le fichier de configuration. Les attributs suivants sont disponibles.

| **Élément** | **Description** |
| --- | --- |
| **Cache** | Élément facultatif. Définit les paramètres de cache d’application globale. |
| **outputCache** | Élément facultatif. Spécifie les paramètres de mise en cache à l’échelle de l’application. |
| **outputCacheSettings** | Élément facultatif. Spécifie les paramètres de sortie-cache qui peuvent être appliqués aux pages de l’application. |
| **sqlCacheDependency** | Élément facultatif. Configure les dépendances de cache SQL pour une application ASP.NET. |

### <a name="the-ltcachegt-element"></a>L’élément &lt;cache&gt;

Les attributs suivants sont &lt;&gt; disponibles dans l’élément cache :

| **Attribut** | **Description** |
| --- | --- |
| **désactiverMemoryCollection** | Attribut **Boolean** facultatif. Obtient ou définit une valeur indiquant si la collection de mémoire de cache qui se produit lorsque la machine est sous pression de mémoire est désactivée. |
| **désactiverExpiration** | Attribut **Boolean** facultatif. Obtient ou définit une valeur indiquant si l’expiration du cache est désactivée. Lorsqu’ils sont désactivés, les éléments mis en cache n’expirent pas et le nettoyage de fond des éléments de cache périmés ne se produit pas. |
| **privateBytesLimit** | Attribut **Int64** facultatif. Obtient ou définit une valeur indiquant la taille maximale des octets privés d’une application avant que le cache commence à rincer les éléments expirés et à tenter de récupérer la mémoire. Cette limite comprend à la fois la mémoire utilisée par le cache ainsi que la mémoire normale au-dessus de l’application en cours d’exécution. Un paramètre de zéro indique que ASP.NET utilisera ses propres heuristiques pour déterminer quand commencer à récupérer la mémoire. |
| **pourcentagePhysicalMemoryUsedLimit** | Attribut **Int32** optionnel. Obtient ou définit une valeur indiquant le pourcentage maximum de la mémoire physique d’une machine qui peut être consommée par une application avant que le cache commence à rincer les éléments expirés et tenter de récupérer la mémoire Cette utilisation de mémoire comprend à la fois la mémoire utilisée par le cache ainsi que l’utilisation normale de la mémoire de l’application en cours d’exécution. Un paramètre de zéro indique que ASP.NET utilisera ses propres heuristiques pour déterminer quand commencer à récupérer la mémoire. |
| **privateBytesPollTime** | Attribut **TimeSpan** optionnel. Obtient ou définit une valeur indiquant l’intervalle de temps entre le sondage pour l’utilisation privée de mémoire des octets de l’application. |

### <a name="the-ltoutputcachegt-element"></a>Le &lt;outputCache&gt; Element

Les attributs suivants sont &lt;disponibles&gt; pour l’élément outputCache.

|       <strong>Attribut</strong>        |                                                                                                                                                                                                                                                       <strong>Description</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Attribut <strong>Boolean</strong> facultatif. Permet/désactive le cache de sortie de la page. En cas d’invalidité, aucune page n’est mise en cache indépendamment des paramètres programmatiques ou déclaratifs. La valeur par défaut est <strong>vraie</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Attribut <strong>Boolean</strong> facultatif. Permet/désactive le cache fragment d’application. En cas d’invalidité, aucune page n’est mise en cache indépendamment de la directive [outputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) ou du profil de mise en cache utilisé. Inclut un en-tête de contrôle de cache indiquant que les serveurs proxy en amont ainsi que les clients du navigateur ne devraient pas tenter de mettre en cache la sortie de la page. La valeur par défaut est <strong>false</strong>.                                                 |
| <strong>envoyerCacheControlHeader</strong> |                                                                                                                                                      Attribut <strong>Boolean</strong> facultatif. Obtient ou définit une valeur indiquant si le <strong>cache-contrôle : l’en-tête privé</strong> est envoyé par le module de cache de sortie par défaut. La valeur par défaut est <strong>false</strong>.                                                                                                                                                      |
|      <strong>ometvaryStar</strong>      | Attribut <strong>Boolean</strong> facultatif. Permet /désactiver l’envoi<strong>d’un Http " Vary: \</fort><em>" en-tête dans la réponse. Avec le paramètre par défaut de faux, un "</em>'Vary: \*" <strong>en-tête est envoyé pour la sortie des pages mises en cache. Lorsque l’en-tête Vary est envoyé, il permet de mettre en cache différentes versions en fonction de ce qui est spécifié dans l’en-tête Vary. Par exemple, <em>Vary: User-Agents</em> stockera différentes versions d’une page en fonction de l’agent utilisateur émettant la demande. La valeur par défaut est fausse</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Le &lt;outputCacheSettings&gt; Element

L’élément &lt;outputCacheSettings&gt; permet la création de profils de cache comme décrit précédemment. Le seul élément &lt;enfant pour l’élément&gt; outputCacheSettings est l’élément &lt;outputCacheProfiles&gt; pour configurer les profils de cache.

### <a name="the-ltsqlcachedependencygt-element"></a>L’élément &lt;sqlCacheDependency&gt;

Les attributs suivants sont &lt;disponibles pour l’élément sqlCacheDependency.&gt;

| **Attribut** | **Description** |
| --- | --- |
| **Activé** | Attribut **Boolean** requis. Indique si des changements sont demandés ou non. |
| **PollTime (en)** | Attribut **Int32** optionnel. Définit la fréquence avec laquelle le SqlCacheDependency sonde le tableau de base de données pour les changements. Cette valeur correspond au nombre de millisecondes entre les sondages successifs. Il ne peut pas être réglé à moins de 500 millisecondes. La valeur par défaut est de 1 minute. |

### <a name="more-information"></a>Informations complémentaires

Il y a des informations supplémentaires que vous devriez être au courant de la configuration du cache.

- Si la limite des octets privés de processus de travail n’est pas définie, le cache utilisera l’une des limites suivantes : 

    - x86 2 Go: 800 Mo ou 60% de RAM physique, selon le moins
    - x86 3 Go: 1800 Mo ou 60% de RAM physique, selon le moins
    - x64: 1 téraoctet ou 60% de RAM physique, selon le moins
- Si les deux travailleurs traitent &lt;la limite des&gt; octets privés et cachent privateBytesLimit/ sont définis, le cache utilisera le minimum des deux.
- Tout comme dans 1.x, nous laissons tomber les entrées de cache et appelons GC. Recueillir pour deux raisons: 

    - Nous sommes très proches de la limite des octets privés
    - La mémoire disponible est proche ou inférieure à 10%
- Vous pouvez désactiver efficacement la garniture et le &lt;cache pour les conditions de mémoire&gt; faibles disponibles en définissant le pourcentage de cachePhysicalMemoryUseLimit/ à 100.
- Contrairement à 1.x, 2.0 suspendra la garniture et recueillera des appels si le dernier GC. Collect n’a pas réduit les octets privés ou la taille des tas gérés de plus de 1% de la limite de mémoire (cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Dépendances de cache personnalisées

1. Créez un nouveau site Web.
2. Ajoutez un nouveau fichier XML appelé cache.xml et enregistrez-le à la racine de l’application Web.
3. Ajoutez le code suivant\_à la méthode de charge de page dans le code-derrière de default.aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Ajoutez ce qui suit au sommet de default.aspx dans la vue source: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Parcourir Default.aspx. Qu’est-ce que le temps dit ?
6. Actualisez le navigateur. Qu’est-ce que le temps dit ?
7. Ouvrez cache.xml et ajoutez le code suivant : 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Enregistrer cache.xml.
9. Actualisez votre navigateur. Qu’est-ce que le temps dit ?
10. Expliquez pourquoi le temps mis à jour au lieu d’afficher les valeurs précédemment mises en cache :

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratoire 2 : Utilisation de dépendances à cache basées sur les sondages

Ce laboratoire utilise le projet que vous avez créé dans le module précédent qui permet l’édition de données dans la base de données Northwind via un contrôle GridView et DetailsView.

1. Ouvrez le projet en Visual Studio 2005.
2. Exécutez l’utilitaire aspnet\_regsql contre la base de données Northwind pour activer la base de données et la table produits. Utilisez la commande suivante à partir d’une invite de commande de studio visuel : 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Ajoutez ce qui suit à votre fichier web.config : 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Ajoutez une nouvelle forme web appelée showdata.aspx.
5. Ajouter la directive suivante sur les résultats à la page showdata.aspx : 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Ajoutez le code suivant\_à la page Charge de showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Ajoutez un nouveau contrôle SqlDataSource à showdata.aspx et configurez-le pour utiliser la connexion de base de données Northwind. Cliquez sur Suivant.
8. Sélectionnez les case box ProductName et ProductID et cliquez sur Next.
9. Cliquez sur Finish.
10. Ajoutez une nouvelle GridView à la page showdata.aspx.
11. Choisissez SqlDataSource1 à partir de la décrocheur.
12. Enregistrer et parcourir showdata.aspx. Prenez note du temps affiché.
