---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configuration et instrumentation Microsoft Docs
author: rick-anderson
description: Il y a des changements majeurs dans la configuration et l’instrumentation dans ASP.NET 2.0. La nouvelle configuration ASP.NET API permet de modifier la configuration...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 30ea2ffd3d055c5373a33615bc19a8f68fb568ab
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543052"
---
# <a name="configuration-and-instrumentation"></a>Configuration et instrumentation

par [Microsoft](https://github.com/microsoft)

> Il y a des changements majeurs dans la configuration et l’instrumentation dans ASP.NET 2.0. La nouvelle configuration ASP.NET API permet d’apporter des modifications de configuration programmatiquement. En outre, de nombreux nouveaux paramètres de configuration existent permettent de nouvelles configurations et l’instrumentation.

Il y a des changements majeurs dans la configuration et l’instrumentation dans ASP.NET 2.0. La nouvelle configuration ASP.NET API permet d’apporter des modifications de configuration programmatiquement. En outre, de nombreux nouveaux paramètres de configuration existent permettent de nouvelles configurations et l’instrumentation.

Dans ce module, nous discuterons ASP.NET la configuration API en ce qui concerne la lecture et l’écriture à ASP.NET fichiers de configuration, et nous couvrirons également ASP.NET’instrumentation. Nous couvrirons également les nouvelles fonctionnalités disponibles dans ASP.NET traçage.

## <a name="aspnet-configuration-api"></a>ASP.NET Configuration API

L’API de configuration ASP.NET vous permet de développer, déployer et gérer les données de configuration d’application en utilisant une interface de programmation unique. Vous pouvez utiliser l’API de configuration pour développer et modifier les configurations complètes ASP.NET programmatiquement sans modifier directement le XML dans les fichiers de configuration. En outre, vous pouvez utiliser l’API de configuration dans les applications de consoles et les scripts que vous développez, dans les outils de gestion Web, et dans Microsoft Management Console (MMC) snap-ins.

Les deux outils de gestion de configuration suivants utilisent l’API de configuration et sont inclus dans la version cadre .NET 2.0 :

- Le ASP.NET MMC snap-in, qui utilise la configuration API pour simplifier les tâches administratives, fournissant une vue intégrée des données de configuration locale de tous les niveaux de la hiérarchie de configuration.
- L’outil d’administration de site Web, qui vous permet de gérer les paramètres de configuration pour les applications locales et distantes, y compris les sites hébergés.

L’API de configuration ASP.NET comprend un ensemble d’objets de gestion ASP.NET que vous pouvez utiliser pour configurer des sites Web et des applications programmatiquement. Les objets de gestion sont mis en œuvre comme une bibliothèque de classe cadre .NET. Le modèle de programmation API de configuration permet d’assurer la cohérence et la fiabilité du code en appliquant les types de données au moment de la compilation. Pour faciliter la gestion des configurations d’applications, l’API de configuration vous permet de visualiser les données qui sont fusionnées à partir de tous les points de la hiérarchie de configuration comme une seule collection, au lieu de considérer les données comme des collections distinctes à partir de différents fichiers de configuration. En outre, la configuration API vous permet de manipuler des configurations d’applications entières sans modifier directement le XML dans les fichiers de configuration. Enfin, l’API simplifie les tâches de configuration en soutenant les outils administratifs, tels que l’outil d’administration des sites Web. La configuration API simplifie le déploiement en soutenant la création de fichiers de configuration sur un ordinateur et en exécutant des scripts de configuration sur plusieurs ordinateurs.

> [!NOTE]
> La configuration API ne prend pas en charge la création d’applications IIS.

## <a name="working-with-local-and-remote-configuration-settings"></a>Travailler avec les paramètres de configuration locale et à distance

Un objet Configuration représente la vue fusionnée des paramètres de configuration qui s’appliquent à une entité physique spécifique, comme un ordinateur, ou à une entité logique, comme une application ou un site Web. L’entité logique spécifiée peut exister sur l’ordinateur local ou sur un serveur distant. Lorsqu’il n’existe pas de fichier de configuration pour une entité spécifiée, l’objet Configuration représente les paramètres de configuration par défaut tels que définis par le fichier Machine.config.

Vous pouvez obtenir un objet Configuration en utilisant l’une des méthodes de configuration ouverte des classes suivantes :

1. La classe ConfigurationManager, si votre entité est une application client.
2. La classe WebConfigurationManager, si votre entité est une application Web.

Ces méthodes retourneront un objet Configuration, qui fournit à son tour les méthodes et les propriétés requises pour gérer les fichiers de configuration sous-jacents. Vous pouvez accéder à ces fichiers pour la lecture ou l’écriture.

### <a name="reading"></a>Lecture

Vous utilisez la méthode GetSection ou GetSectionGroup pour lire les informations de configuration. L’utilisateur ou le processus qui se lit doit avoir des autorisations de lecture sur tous les fichiers de configuration dans la hiérarchie.

> [!NOTE]
> Si vous utilisez une méthode GetSection statique qui prend un paramètre de trajectoire, le paramètre de trajectoire doit se référer à l’application dans laquelle le code est en cours d’exécution. Dans le cas contraire, le paramètre est ignoré et les informations de configuration pour l’application en cours d’exécution sont retournées.

### <a name="writing"></a>Écriture

Vous utilisez l’une des méthodes Enregistrer pour écrire des informations de configuration. L’utilisateur ou le processus qui écrit doit avoir des autorisations d’écriture sur le fichier de configuration et l’annuaire au niveau actuel de la hiérarchie de configuration, ainsi que des autorisations de lecture sur tous les fichiers de configuration dans la hiérarchie.

Pour générer un fichier de configuration qui représente les paramètres de configuration hérités d’une entité spécifiée, utilisez l’une des méthodes de configuration d’enregistrement suivantes :

1. La méthode Save pour créer un nouveau fichier de configuration.
2. La méthode SaveAs pour générer un nouveau fichier de configuration à un autre endroit.

## <a name="configuration-classes-and-namespaces"></a>Classes de configuration et espaces de noms

De nombreuses classes et méthodes de configuration sont similaires les unes aux autres. Le tableau suivant décrit les classes de configuration et les espaces de nom les plus couramment utilisés.

| **Classe de configuration ou espace de nom** | **Description** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) namespace (en) | Contient les principales classes de configuration pour toutes les applications cadre .NET. Les classes de gestionnaires de section sont utilisées pour obtenir des données de configuration pour une section à partir de méthodes telles que GetSection et GetSectionGroup. Ces deux méthodes ne sont pas statiques. |
| Classe System.Configuration.Configuration | Représente un ensemble de données de configuration pour un ordinateur, une application, un annuaire Web ou une autre ressource. Cette classe contient des méthodes utiles, telles que GetSection et GetSectionGroup, pour la mise à jour des paramètres de configuration et l’obtention de références aux sections et aux groupes de section. Cette classe est utilisée comme type de retour pour les méthodes qui obtiennent des données de configuration de conception-temps, telles que les méthodes des classes WebConfigurationManager et ConfigurationManager. |
| System.Web.Configuration namespace (en) | Contient les classes de gestionnaire de section pour les sections de configuration ASP.NET définies [à ASP.NET Paramètres de configuration](https://msdn.microsoft.com/library/b5ysx397.aspx). Les classes de gestionnaires de section sont utilisées pour obtenir des données de configuration pour une section à partir de méthodes telles que GetSection et GetSectionGroup. |
| System.Web.Configuration.WebConfigurationManager classe | Fournit des méthodes utiles pour obtenir des références aux paramètres de configuration en temps de course et en temps de conception. Ces méthodes utilisent la classe System.Configuration.Configuration comme type de retour. Vous pouvez utiliser la méthode GetSection statique de cette classe ou la méthode GetSection non statique de la classe System.Configuration.ConfigurationManager de manière interchangeable. Pour les configurations d’applications Web, la classe System.Web.Configuration.WebConfigurationManager est recommandée au lieu de la classe System.Configuration.ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) namespace (en) | Fournit un moyen de personnaliser et d’étendre le fournisseur de configuration. Il s’agit de la classe de base pour toutes les classes de fournisseurs dans le système de configuration. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) namespace (en) | Contient des classes et des interfaces pour la gestion et le suivi de la santé des applications Web. Strictement parlant, cet espace de nom n’est pas considéré comme faisant partie de la configuration API. Par exemple, le traçage et le tir d’événements sont accomplis par les classes dans cet espace de nom. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) namespace (en) | Fournit les classes nécessaires à l’instrumentation des applications pour exposer leurs informations de gestion et événements par l’intermédiaire de Windows Management Instrumentation (WMI) aux consommateurs potentiels. ASP.NET la surveillance de la santé utilise l’IMM pour fournir des événements. Strictement parlant, cet espace de nom n’est pas considéré comme faisant partie de la configuration API. |

## <a name="reading-from-aspnet-configuration-files"></a>Lecture des fichiers de configuration ASP.NET

La classe WebConfigurationManager est la classe de base pour la lecture à partir de fichiers de configuration ASP.NET. Il y a essentiellement trois étapes pour lire ASP.NET fichiers de configuration :

1. Obtenez un objet Configuration en utilisant la méthode OpenWebConfiguration.
2. Obtenez une référence à la section désirée dans le fichier de configuration.
3. Lisez les informations souhaitées à partir du fichier de configuration.

L’objet Configuration représente ne représente pas un fichier de configuration particulier. Au lieu de cela, il représente une vue fusionnée de la configuration d’un ordinateur, d’une application ou d’un site Web. L’échantillon de code suivant instantané un objet Configuration représentant la configuration d’une application Web appelée *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Notez que si le parcours /ProductInfo n’existe pas, le code ci-dessus retournera la configuration par défaut comme indiqué dans le fichier machine.config.

Une fois que vous avez l’objet Configuration, vous pouvez ensuite utiliser la méthode GetSection ou GetSectionGroup pour percer dans les paramètres de configuration. L’exemple suivant fait référence aux paramètres d’usurpation d’identité pour l’application ProductInfo ci-dessus :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Rédaction de fichiers de configuration ASP.NET

Comme dans la lecture des fichiers de configuration, la classe WebConfigurationManager est le noyau pour l’écriture pour Asp.NET fichiers de configuration. Il ya aussi trois étapes à l’écriture pour ASP.NET fichiers de configuration.

1. Obtenez un objet Configuration en utilisant la méthode OpenWebConfiguration.
2. Obtenez une référence à la section désirée dans le fichier de configuration.
3. Écrivez les informations souhaitées à partir du fichier de configuration à l’aide de la méthode Save or SaveAs.

Le code suivant modifie l’attribut &lt;&gt; **de débogé** de l’élément de compilation en faux :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Lorsque ce code est exécuté, **l’attribut de déboug de** &lt;&gt; l’élément compilation sera mis à faux pour le fichier web.config de l’application *webApp.*

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

L’espace de nom System.Web.Management fournit les classes et les interfaces pour gérer et surveiller la santé des applications ASP.NET.

L’exploitation forestière est accomplie en définissant une règle qui associe les événements à un fournisseur. La règle définit le type d’événements qui sont envoyés au fournisseur. Les événements de base suivants sont disponibles pour vous connecter :

| **WebBaseEvent (en)** | La classe d’événements de base pour tous les événements. Contient les propriétés requises pour tous les événements tels que le code d’événement, le code de détail de l’événement, la date et l’heure de l’événement a été soulevée, le numéro de séquence, le message de l’événement et les détails de l’événement. |
| --- | --- |
| **WebManagementEvent** | La classe d’événements de base pour les événements de gestion, tels que la durée de vie de l’application, la demande, l’erreur et les événements de vérification. |
| **WebHeartbeatEvent (en)** | L’événement généré par l’application à intervalles réguliers pour capturer des informations utiles sur l’état d’exécution. |
| **WebAuditEvent (en)** | La classe de base pour les événements d’audit de sécurité, qui sont utilisés pour marquer des conditions telles que la défaillance de l’autorisation, la défaillance du décryptage, *etc.* |
| **WebRequestEvent (en)** | La classe de base pour tous les événements de demande d’information. |
| **WebBaseErrorEvent (en)** | La classe de base pour tous les événements indiquant les conditions d’erreur. |

Les types de fournisseurs disponibles vous permettent d’envoyer la sortie de l’événement à Event Viewer, SQL Server, Windows Management Instrumentation (WMI) et e-mail. Les fournisseurs préconfigurés et les cartes d’événements réduisent la quantité de travail nécessaire pour obtenir la sortie de l’événement enregistrée.

ASP.NET 2.0 utilise le fournisseur Event Log out-of-the-box pour enregistrer des événements basés sur les domaines d’application de démarrage et d’arrêt, ainsi que l’enregistrement de toute exception non gérée. Cela permet de couvrir certains des scénarios de base. Par exemple, disons que votre application lance une exception, mais l’utilisateur n’enregistre pas l’erreur et vous ne pouvez pas la reproduire. Avec la règle par défaut Event Log, vous seriez en mesure de recueillir l’exception et empiler des informations pour avoir une meilleure idée de ce type d’erreur s’est produite. Un autre exemple s’applique si votre demande perd l’état de session. Dans ce cas, vous pouvez regarder dans le journal d’événements pour déterminer si le domaine d’application est le recyclage, et pourquoi le domaine d’application arrêté en premier lieu.

En outre, le système de surveillance de la santé est extensible. Par exemple, vous pouvez définir des événements Web personnalisés, les virer dans votre application, puis définir une règle pour envoyer les informations de l’événement à un fournisseur tel que votre e-mail. Cela vous permet de lier facilement votre instrumentation aux fournisseurs de surveillance de la santé. Par exemple, vous pouvez congédier un événement chaque fois qu’une commande est traitée et configurer une règle qui envoie chaque événement à la base de données SQL Server. Vous pouvez également allumer un événement lorsqu’un utilisateur ne parvient pas à vous connecter plusieurs fois de suite et configurer l’événement pour utiliser les fournisseurs de messagerie.

La configuration pour les fournisseurs par défaut et les événements est stockée dans le fichier Web.config global. Le fichier Web.config mondial stocke tous les paramètres Web qui ont été stockés dans le fichier Machine.config dans ASP.NET 1x. Le fichier Web.config global est situé dans l’annuaire suivant :

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

La &lt;section healthMonitoring&gt; du fichier Web.config mondial fournit des paramètres de configuration par défaut. Vous pouvez remplacer ces paramètres ou configurer &lt;vos propres&gt; paramètres en implémentant la section healthMonitoring dans le fichier Web.config pour votre application.

La &lt;section healthMonitoring&gt; du fichier Web.config mondial contient les éléments suivants :

| **fournisseurs** | Contient des fournisseurs mis en place pour le Visual Event Viewer, WMI et SQL Server. |
| --- | --- |
| **eventMappings** | Contient des cartes pour les différentes classes WebBase. Vous pouvez étendre cette liste si vous génèrez votre propre classe d’événements. Générer votre propre classe d’événements vous donne une granularité plus fine sur les fournisseurs que vous envoyez des informations à. Par exemple, vous pouvez configurer des exceptions non manipulées à envoyer à SQL Server, tout en envoyant vos propres événements personnalisés par e-mail. |
| **Règles** | Liens les événementsMappings au fournisseur. |
| **Tampon** | Utilisé avec SQL Server et les fournisseurs de messagerie pour déterminer à quelle fréquence de rincer les événements au fournisseur. |

Voici un exemple de code du fichier Web.config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Comment stocker des événements à Event Viewer

Comme mentionné précédemment, le fournisseur d’événements de connexion dans le Visual Event est configuré pour vous dans le fichier Web.config mondial. Par défaut, tous les événements basés sur **WebBaseErrorEvent** et **WebFailureAuditEvent** sont enregistrés. Vous pouvez ajouter des règles supplémentaires pour enregistrer des informations supplémentaires au journal des événements. Par exemple, si vous voulez enregistrer tous les événements *(c’est-à-dire,* chaque événement basé sur **WebBaseEvent**), vous pouvez ajouter la règle suivante à votre fichier Web.config :

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Cette règle relierait la carte de **l’événement All Events** au fournisseur de journaux d’événements. Les deux eventMapping et le fournisseur sont inclus dans le fichier Web.config mondial.

## <a name="how-to-store-events-to-sql-server"></a>Comment stocker des événements à SQL Server

Cette méthode utilise la base de données **ASPNETDB,** qui est générée par l’outil Aspnet\_regsql.exe. Le fournisseur par défaut utilise la chaîne de connexion LocalSqlServer,\_qui utilise soit une base de données basée sur des fichiers dans le dossier de données App ou l’instance SQLExpress locale de SQL Server. La chaîne de connexion LocalSqlServer et le SqlProvider sont configurés dans le fichier Web.config global.

La chaîne de connexion LocalSqlServer dans le fichier Web.config mondial ressemble à ceci :

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Si vous souhaitez utiliser une autre instance SQL Server, vous\_devrez utiliser l’outil Aspnet regsql.exe, qui peut être trouvé dans le %windir%-Microsoft.Net-Framework.v2.0. \* dossier. Utilisez l’outil\_Aspnet regsql.exe pour générer une base de données **ASPNETDB** personnalisée sur l’instance SQL Server, puis ajoutez la chaîne de connexion à votre fichier de configuration d’applications, puis ajoutez un fournisseur en utilisant la nouvelle chaîne de connexion. Une fois que vous avez créé la base de données **ASPNETDB,** vous devrez définir une règle pour lier un eventMapping au sqlProvider.

Que vous utilisiez le SqlProvider par défaut ou que vous configuriez votre propre fournisseur, vous devrez ajouter une règle reliant le fournisseur à une carte d’événement. La règle suivante relie le nouveau fournisseur que vous avez créé ci-dessus à la carte de l’événement **All Events.** Cette règle enregistrera tous les événements basés sur **WebBaseEvent** et les enverra au MySqlWebEventProvider qui utilisera la chaîne de connexion MYASPNETDB. Le code suivant ajoute une règle pour lier le fournisseur à une carte d’événement :

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Si vous ne vouliez envoyer que des erreurs à SQL Server, vous pouvez ajouter la règle suivante :

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Comment transmettre les événements à WMI

Vous pouvez également transmettre les événements à WMI. Le fournisseur WMI est configuré pour vous dans le fichier Web.config global par défaut.

L’exemple de code suivant ajoute une règle pour transmettre les événements à WMI :

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Vous devrez ajouter une règle pour associer un eventMapping au fournisseur, ainsi qu’une application d’écoute WMI pour écouter les événements. L’exemple de code suivant ajoute une règle pour lier le fournisseur WMI à la carte de **l’événement All Events** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Comment transmettre des événements à l’email

Vous pouvez également transmettre des événements à l’email. Faites attention aux règles d’événement que vous cartographiez à votre fournisseur de messagerie, car vous pouvez involontairement vous envoyer beaucoup d’informations qui peuvent être mieux adaptées pour SQL Server ou le journal d’événements. Il y a deux fournisseurs de messagerie; SimpleMailWebEventProvider et TemplatedMailWebEventProvider. Chacun a les mêmes attributs de configuration, à l’exception des attributs "template" et "detailedTemplateErrors", qui ne sont disponibles que sur le TemplatedMailWebEventProvider.

> [!NOTE]
> Aucun de ces fournisseurs de messagerie n’est configuré pour vous. Vous devrez les ajouter à votre fichier Web.config.

La principale différence entre ces deux fournisseurs de messagerie est que SimpleMailWebEventProvider envoie des e-mails dans un modèle générique qui ne peut pas être modifié. Le fichier Web.config de l’échantillon ajoute ce fournisseur de messagerie à la liste des fournisseurs configurés en utilisant la règle suivante :

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

La règle suivante est également ajoutée pour lier le fournisseur de messagerie à la carte de l’événement **All Events** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 Traçage

Il y a trois améliorations majeures à tracer dans ASP.NET 2.0.

1. Fonctionnalité de traçage intégrée
2. Accès programmatique aux messages de traçabilité
3. Amélioration du traçage au niveau de l’application

## <a name="integrated-tracing-functionality"></a>Fonctionnalité de traçage intégrée

Vous pouvez maintenant acheminer les messages émis par la classe System.Diagnostics.Trace pour ASP.NET la sortie de traçage, et les messages d’itinéraire émis par ASP.NET traçant à System.Diagnostics.Trace. Vous pouvez également transmettre ASP.NET événements d’instrumentation à System.Diagnostics.Trace. Cette fonctionnalité est fournie par la nouvelle **écritureToDiagnosticsTrace** attribut de l’élément &lt;trace.&gt; Lorsque cette valeur Boolean est vraie, ASP.NET messages Trace sont transmis à l’infrastructure de traçage System.Diagnostics pour une utilisation par tous les auditeurs qui sont enregistrés pour afficher des messages Trace.

## <a name="programmatic-access-to-trace-messages"></a>Accès programmatique aux messages de trace

ASP.NET 2.0 permet un accès programmatique à tous les messages de trace via la classe **TraceContextRecord** et la collection **TraceRecords.** Le moyen le plus efficace d’accéder aux messages de trace est d’enregistrer un délégué **TraceContextEventHandler** (également nouveau en ASP.NET 2.0) pour gérer le nouvel événement **TraceFinished.** Vous pouvez ensuite boucler les messages de trace comme vous le souhaitez.

L’échantillon de code suivant illustre ceci :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Dans l’exemple ci-dessus, je boucle à travers la collection TraceRecords, puis écrire chaque message sur le flux de réponse.

## <a name="improved-application-level-tracing"></a>Amélioration de la traçabilité au niveau de l’application

Le traçage au niveau de l’application est amélioré &lt;&gt; par l’introduction du nouvel attribut le **plus récent** de l’élément trace. Cet attribut précise si la plus récente sortie de traçage au niveau de l’application est affichée et que les données de trace plus anciennes au-delà des limites indiquées par la demandelimit sont rejetées. En cas de faux, des données de trace sont affichées pour les demandes jusqu’à ce que l’attribut De la demandeLimit soit atteint.

## <a name="aspnet-command-line-tools"></a>outils de ligne de commandement ASP.NET

Il existe plusieurs outils de ligne de commande pour faciliter la configuration des ASP.NET. ASP.NET développeurs doivent être familiers avec l’outil aspnet\_regiis.exe. ASP.NET 2.0 fournit trois autres outils de ligne de commande pour faciliter la configuration.

Les outils de ligne de commande suivants sont disponibles :

| **Outil** | **Utilisation** |
| --- | --- |
| **aspnet\_regiis.exe** | Permet l’enregistrement des ASP.NET avec l’IIS. Il existe deux versions de ces outils qui expédient avec ASP.NET 2.0, une pour les systèmes 32 bits (dans le dossier Framework) et une pour les systèmes 64 bits (dans le dossier Framework64). La version 64 bits ne sera pas installée sur un système d’exploitation 32 bits. |
| **aspnet\_regsql.exe** | L’outil d’enregistrement des serveurs SQL ASP.NET est utilisé pour créer une base de données Microsoft SQL Server pour une utilisation par les fournisseurs de serveurs SQL dans ASP.NET, ou pour ajouter ou supprimer des options d’une base de données existante. Le fichier\_Aspnet regsql.exe est situé dans le dossier [drive:]-WINDOWS.Microsoft.NET-Framework-versionNumber sur votre serveur Web. |
| **aspnet\_regbrowsers.exe** | L’outil d’enregistrement de navigateur ASP.NET analyse et compile toutes les définitions de navigateur à l’échelle du système dans un assemblage et installe l’assemblage dans le cache d’assemblage global. L’outil utilise les fichiers de définition du navigateur (. Fichiers BROWSER) à partir de la sous-direction .NET Framework Browsers. L’outil se trouve dans l’annuaire %SystemRoot%-Microsoft.NET-Framework.version. |
| **compiler\_aspnet.exe** | L’outil ASP.NET Compilation vous permet de compiler une application Web ASP.NET, en place ou pour le déploiement à un emplacement cible tel qu’un serveur de production. La compilation place permet d’atteindre l’application parce que les utilisateurs finaux ne rencontrent pas de retard sur la première demande de l’application pendant que l’application est compilée. |

Parce que l’outil aspnet\_regiis.exe n’est pas nouveau pour ASP.NET 2.0, nous n’en discuterons pas ici.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>ASP.NET outil d’enregistrement des serveurs\_SQL - aspnet regsql.exe

Vous pouvez définir plusieurs types d’options à l’aide de l’outil d’enregistrement des serveurs SQL ASP.NET. Vous pouvez spécifier une connexion SQL, spécifier quels ASP.NET services d’application utilisent SQL Server pour gérer les informations, indiquer quelle base de données ou table est utilisée pour la dépendance au cache SQL, et ajouter ou supprimer la prise en charge de l’utilisation du serveur SQL pour stocker les procédures et l’état de la session.

Plusieurs ASP.NET services d’application comptent sur un fournisseur pour gérer le stockage et la récupération des données à partir d’une source de données. Chaque fournisseur est spécifique à la source de données. ASP.NET comprend un fournisseur de serveur SQL pour les fonctionnalités ASP.NET suivantes :

- Adhésion (la classe [SqlMembershipProvider).](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)
- Gestion des rôles (classe [SqlRoleProvider).](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)
- Profil (la classe [SqlProfileProvider).](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx)
- Personnalisation des pièces Web (la classe [SqlPersonalizationProvider).](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx)
- Événements Web (la classe [SqlWebEventProvider).](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx)

Lorsque vous installez ASP.NET, le fichier Machine.config pour votre serveur comprend des éléments de configuration qui spécifient les fournisseurs de serveurs SQL pour chacune des fonctionnalités ASP.NET qui s’appuient sur un fournisseur. Ces fournisseurs sont configurés, par défaut, pour se connecter à une instance utilisateur locale de SQL Server Express 2005. Si vous modifiez la chaîne de connexion par défaut utilisée par les fournisseurs, alors avant de pouvoir utiliser l’une des fonctionnalités ASP.NET configurées dans la configuration de la machine, vous devez installer la base de données SQL Server et les éléments de base de données pour votre fonctionnalité choisie à l’aide d’Aspnet\_regsql.exe. Si la base de données que vous spécifiez avec l’outil d’enregistrement SQL n’existe pas déjà (aspnetdb sera la base de données par défaut si l’on n’est pas spécifié sur la ligne de commande), alors l’utilisateur actuel doit avoir les droits de créer des bases de données dans SQL Server ainsi que de créer des éléments schéma dans une base de données.

### <a name="sql-cache-dependency"></a>Dépendance de cache SQL

Une caractéristique avancée de la mise en cache de sortie ASP.NET est la dépendance à la cache SQL. La dépendance au cache SQL prend en charge deux modes de fonctionnement différents : l’un qui utilise une mise en œuvre ASP.NET des sondages de table et un deuxième mode qui utilise les fonctions de notification de requête de SQL Server 2005. L’outil d’enregistrement SQL peut être utilisé pour configurer le mode de fonctionnement du classement.

### <a name="session-state"></a>État de la session

Par défaut, les valeurs et les informations de l’état de session sont stockées dans la mémoire dans le processus ASP.NET. Alternativement, vous pouvez stocker des données de session dans une base de données SQL Server, où ils peuvent être partagés par plusieurs serveurs Web. Si la base de données que vous spécifiez pour l’état de session avec l’outil d’enregistrement SQL n’existe pas déjà, alors l’utilisateur actuel doit avoir le droit de créer des bases de données dans SQL Server ainsi que de créer des éléments schémas dans une base de données. Si la base de données existe, l’utilisateur actuel doit avoir le droit de créer des éléments de schéma dans la base de données existante.

Pour installer la base de données d’état\_de session sur SQL Server, exécutez l’outil Aspnet regsql.exe et fournissez les informations suivantes avec la commande :

- Le nom de l’instance SQL Server, en utilisant l’option **-S.**
- Les informations d’identification logon pour un compte qui a la permission de créer une base de données sur un ordinateur fonctionnant SQL Server. Utilisez l’option **-E** pour utiliser l’utilisateur actuellement connecté, ou utilisez l’option **-U** pour spécifier un identifiant d’utilisateur ainsi que l’option **-P** pour spécifier un mot de passe.
- L’option de ligne de **commande-ssadd** pour ajouter la base de données d’état de session.

Par défaut, vous ne pouvez\_pas utiliser l’outil Aspnet regsql.exe pour installer la base de données d’état de session sur un ordinateur exécutant SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>L’outil d’enregistrement des navigateurs\_ASP.NET - aspnet regbrowsers.exe

Dans ASP.NET version 1.1, le fichier Machine.config &lt;contenait&gt;une section appelée browserCaps . Cette section contenait une série d’entrées XML qui définissaient les configurations pour divers navigateurs en fonction d’une expression régulière. Pour ASP.NET version 2.0, une nouvelle . Le fichier BROWSER définit les paramètres d’un navigateur particulier à l’aide d’entrées XML. Vous ajoutez des informations sur un nouveau navigateur en ajoutant un nouveau . Fichier BROWSER au dossier situé à %SystemRoot%-Microsoft.NET-Framework-version-CONFIG-Browsers sur votre système.

Parce qu’une application ne lit pas un fichier .config chaque fois qu’elle nécessite des informations de navigateur, vous pouvez créer un nouveau . Fichier BROWSER et exécuter\_Aspnet regbrowsers.exe pour ajouter les modifications requises à l’assemblage. Cela permet au serveur d’accéder immédiatement aux nouvelles informations du navigateur afin que vous n’ayez pas à arrêter l’une de vos applications pour récupérer les informations. Une application peut accéder aux capacités du navigateur via la propriété Browser de l’actuelle HttpRequest.

Les options suivantes sont disponibles\_lors de l’exécution aspnet regbrowser.exe:

| **Option** | **Description** |
| --- | --- |
| **-?** | Affiche le texte\_Aspnet regbbrowsers.exe Aide dans la fenêtre de commande. |
| **-i** | Crée l’assemblage des capacités du navigateur en temps d’exécution et l’installe dans le cache d’assemblage global. |
| **-u** | Désinstalle l’assemblage des capacités du navigateur en temps d’exécution à partir du cache d’assemblage global. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>L’outil de compilation ASP.NET -\_aspnet compiler.exe

L’outil ASP.NET Compilation peut être utilisé de deux manières générales : pour la compilation et la compilation sur place pour le déploiement, où un répertoire de sortie cible est spécifié.

### <a name="compiling-an-application-in-place"></a>[Compilation d’une demande en place](https://msdn.microsoft.com/library/ms229863.aspx)

L’outil ASP.NET Compilation peut compiler une application en place, c’est-à-dire qu’il imite le comportement de faire plusieurs demandes à l’application, provoquant ainsi une compilation régulière. Les utilisateurs d’un site pré-compilé ne connaîtront pas de retard causé par la compilation de la page à la première demande.

Lorsque vous prépilposez un site en place, les éléments suivants s’appliquent :

- Le site conserve ses fichiers et sa structure d’annuaire.
- Vous devez avoir des compilateurs pour tous les langages de programmation utilisés par le site sur le serveur.
- Si un fichier échoue à la compilation, l’ensemble du site échoue compilation.

Vous pouvez également recomposer une application en place après y avoir ajouté de nouveaux fichiers sources. L’outil ne compile que les fichiers nouveaux ou modifiés à moins que vous incluiez l’option **-c.**

> [!NOTE]
> La compilation d’une application qui contient une application imbriquée ne compile pas l’application imbriquée. L’application imbriquée doit être compilée séparément.

### <a name="compiling-an-application-for-deployment"></a>[Compilation d’une demande de déploiement](https://msdn.microsoft.com/library/ms229863.aspx)

Vous compilez une application de déploiement (compilation à un emplacement cible) en spécifiant le paramètre targetDir. Le targetDir peut être l’emplacement final de l’application Web, ou l’application compilée peut être déployée plus loin. L’utilisation **de l’option -u** compile l’application de telle sorte que vous pouvez apporter des modifications à certains fichiers de l’application compilée sans la recompliser. Aspnet\_compiler.exe fait une distinction entre les types de fichiers statiques et dynamiques, et les gère différemment lors de la création de l’application résultante.

- Les types de fichiers statiques sont ceux qui n’ont pas de compilateur ou de fournisseur de construction associé, tels que les fichiers dont les noms ont des extensions telles que .css, .gif, .htm, .html, .jpg, .js et ainsi de suite. Ces fichiers sont simplement copiés à l’emplacement cible, avec leurs places relatives dans la structure de l’annuaire préservé.
- Les types de fichiers dynamiques sont ceux qui ont un compilateur ou un fournisseur de construction associé, y compris les fichiers avec des extensions de nom de fichier spécifiques à ASP.NET telles que .asax, .ascx, .ashx, .aspx, .browser, .master, et ainsi de suite. L’outil ASP.NET Compilation génère des assemblages à partir de ces fichiers. Si l’option **-u** est omise, l’outil crée également des fichiers avec l’extension du nom de fichier . COMPILED qui cartographient les fichiers source d’origine à leur assemblage. Pour s’assurer que la structure d’annuaire de la source d’application est préservée, l’outil génère des fichiers de placeholder dans les emplacements correspondants dans l’application cible.

Vous devez utiliser l’option **-u** pour indiquer que le contenu de l’application compilée peut être modifié. Dans le cas contraire, les modifications ultérieures sont ignorées ou causent des erreurs de temps d’exécution.

Le tableau suivant décrit comment l’outil ASP.NET Compilation gère différents types de fichiers lorsque l’option **-u** est incluse.

| **Type de fichier** | **Action Compilateur** |
| --- | --- |
| .ascx, .aspx, .master | Ces fichiers sont divisés en code de balisage et source, qui comprend &lt;à la fois&gt; des fichiers de code et tout code qui est inclus dans le script runat "serveur" éléments. Le code source est compilé dans les assemblages, avec des noms dérivés d’un algorithme de hachage, et les assemblages sont placés dans le répertoire Bin. Tout code en ligne, c’est-à-dire code clos entre les ** &lt; ** ** % ** parenthèses, est inclus avec la majoration et non compilé. De nouveaux fichiers du même nom que les fichiers sources sont créés pour contenir la majoration et placés dans les répertoires de sortie correspondants. |
| .ashx, .asmx | Ces fichiers ne sont pas compilés et sont transférés aux répertoires de sortie tels qu’ils sont compilés et non compilés. Si vous souhaitez que le code de gestionnaire soit compilé,\_placez le code dans les fichiers de code source dans l’annuaire App Code. |
| .cs, .vb, .jsl, .cpp (sans compter les fichiers de code-derrière pour les types de fichiers énumérés plus tôt) | Ces fichiers sont compilés et inclus comme une ressource dans les assemblages qui les renvoient. Les fichiers source ne sont pas copiés au répertoire de sortie. Si un fichier de code n’est pas référencé, il n’est pas compilé. |
| Types de fichiers personnalisés | Ces fichiers ne sont pas compilés. Ces fichiers sont copiés aux annuaires de sortie correspondants. |
| Fichiers de code\_source dans la sous-direction du Code d’application | Ces fichiers sont compilés dans des assemblages et placés dans le répertoire Bin. |
| .resx et .ressources dans\_le sous-direction de App GlobalResources | Ces fichiers sont compilés dans des assemblages et placés dans le répertoire Bin. Aucune\_sous-direction d’App GlobalResources n’est créée sous l’annuaire principal de sortie, et aucun fichier .resx ou .resources situé dans l’annuaire source n’est copié aux annuaires de sortie. |
| .resx et .ressources dans\_le sous-direction de App LocalResources | Ces fichiers ne sont pas compilés et sont copiés aux répertoires de sortie correspondants. |
| .fichiers skin dans\_la sous-direction des thèmes d’application | Les fichiers .skin et les fichiers thématiques statiques ne sont pas compilés et sont copiés aux répertoires de sortie correspondants. |
| .browser Web.config Static types de fichiers Assemblages déjà présents dans le répertoire Bin | Ces fichiers sont copiés comme c’est le fait pour les annuaires de sortie. |

Le tableau suivant décrit comment l’outil ASP.NET Compilation gère différents types de fichiers lorsque l’option **-u** est omise.

| **Type de fichier** | **Action Compilateur** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Ces fichiers sont divisés en code de balisage et source, qui comprend &lt;à la fois&gt; des fichiers de code et tout code qui est inclus dans le script runat "serveur" éléments. Le code source est compilé dans les assemblages, avec des noms dérivés d’un algorithme de hachage. Les assemblages qui en résultent sont placés dans le répertoire Bin. Tout code en ligne, c’est-à-dire code clos entre les ** &lt; ** ** % ** parenthèses, est inclus avec la majoration et non compilé. Le compilateur crée de nouveaux fichiers pour contenir la balisage avec le même nom que les fichiers source. Ces fichiers qui en résultent sont placés dans le répertoire Bin. Le compilateur crée également des fichiers avec le même nom que les fichiers source, mais avec l’extension . COMPILED qui contiennent des informations cartographiques. Lla. Les fichiers COMPILED sont placés dans les répertoires de sortie correspondant à l’emplacement d’origine des fichiers source. |
| .ascx | Ces fichiers sont divisés en balisage et en code source. Le code source est compilé dans les assemblages et placé dans le répertoire Bin, avec des noms qui sont dérivés d’un algorithme de hachage. Aucun fichier de balisage n’est généré. |
| .cs, .vb, .jsl, .cpp (sans compter les fichiers de code-derrière pour les types de fichiers énumérés plus tôt) | Le code source qui est référencé par les assemblages générés à partir de fichiers .ascx, .ashx, ou .aspx est compilé dans les assemblages et placé dans le répertoire Bin. Aucun fichier source n’est copié. |
| Types de fichiers personnalisés | Ces fichiers sont compilés comme des fichiers dynamiques. Selon le type de fichier sur lequel ils sont basés, le compilateur peut placer des fichiers de cartographie dans les répertoires de sortie. |
| Fichiers dans\_la sous-direction du Code d’application | Les fichiers de code source de cette sous-direction sont compilés dans des assemblages et placés dans le répertoire Bin. |
| Fichiers dans\_la sous-direction de App GlobalResources | Ces fichiers sont compilés dans des assemblages et placés dans le répertoire Bin. Aucune\_sous-direction d’App GlobalResources n’est créée sous le répertoire principal de sortie. Si le fichier de configuration spécifie s’appliqueTo "Tous", .resx et .ressources fichiers sont copiés aux répertoires de sortie. Ils ne sont pas copiés s’ils sont référencés par un [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| .resx et .ressources dans\_le sous-direction de App LocalResources | Ces fichiers sont compilés dans des assemblages avec des noms uniques et placés dans le répertoire Bin. Aucun fichier .resx ou .resource n’est copié aux annuaires de sortie. |
| .fichiers skin dans\_la sous-direction des thèmes d’application | Les thèmes sont compilés en assemblées et placés dans le répertoire Bin. Les fichiers Stub sont créés pour les fichiers .skin et placés dans le répertoire de sortie correspondant. Les fichiers statiques (tels que .css) sont copiés aux répertoires de sortie. |
| .browser Web.config Static types de fichiers Assemblages déjà présents dans le répertoire Bin | Ces fichiers sont copiés comme c’est le fait pour le répertoire de sortie. |

### <a name="fixed-assembly-names"></a>[Noms d’assemblage fixes](https://msdn.microsoft.com/library/ms229863.aspx##)

Certains scénarios, tels que le déploiement d’une application Web à l’aide de l’installateur Windows MSI, nécessitent l’utilisation de noms et de contenus de fichiers cohérents, ainsi que des structures d’annuaire cohérentes pour identifier les assemblages ou les paramètres de configuration pour les mises à jour. Dans ces cas, vous pouvez utiliser l’option **-noms fixes** pour spécifier que l’outil ASP.NET Compilation doit compiler un assemblage pour chaque fichier source au lieu d’utiliser l’endroit où plusieurs pages sont compilées dans des assemblages. Cela peut conduire à un grand nombre d’assemblages, donc si vous êtes préoccupé par l’évolutivité, vous devriez utiliser cette option avec prudence.

### <a name="strong-name-compilation"></a>[Compilation de nom fort](https://msdn.microsoft.com/library/ms229863.aspx##)

Les options **-aptca**, **-delaysign**, **-keycontainer** et **-keyfile** sont\_fournies afin que vous puissiez utiliser Aspnet compiler.exe pour créer des assemblages fortement nommés sans utiliser [l’outil de nom fort (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) séparément. Ces options correspondent, respectivement, à **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**, et **AssemblyKeyFileAttribute**.

La discussion de ces attributs n’est pas du tout dans le cadre de ce cours.

## <a name="labs"></a>Laboratoires

Chacun des laboratoires suivants s’appuie sur les laboratoires précédents. Vous devrez les faire dans l’ordre.

## <a name="lab-1-using-the-configuration-api"></a>Laboratoire 1 : Utilisation de l’API Configuration

1. Créer un nouveau site Web appelé *mod9lab*.
2. Ajoutez un nouveau fichier de configuration Web au site.
3. Ajoutez ce qui suit au fichier web.config :

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Cela garantira que vous avez la permission d’enregistrer les modifications apportées au fichier web.config.

1. Ajoutez un nouveau contrôle d’étiquette à Default.aspx et changez l’ID pour **lblDebugStatus**.
2. Ajoutez un nouveau contrôle bouton à Default.aspx.
3. Modifier l’ID du contrôle du bouton en **btnToggleDebug** et le texte pour **basculer le statut de débogé .**
4. Ouvrez la vue de code pour le fichier de code-derrière de Default.aspx et ajoutez une déclaration **utilisant** pour **System.Web.Configuration** comme suit :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Ajoutez deux variables privées à\_la classe et une méthode Page Init comme indiqué ci-dessous :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Ajoutez le code\_suivant à la charge de page :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Enregistrer et parcourir default.aspx. Notez que le contrôle de l’étiquette affiche l’état actuel de débogé.
2. Double-clic sur le contrôle de bouton dans le concepteur et ajouter le code suivant à l’événement Cliquez pour le contrôle de bouton :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Enregistrer et parcourir default.aspx et cliquez sur le bouton.
2. Ouvrez le fichier web.config après chaque clic de &lt;bouton&gt; et observez **l’attribut de débogé** dans la section compilation.

## <a name="lab-2-logging-application-restarts"></a>Lab 2 : Redémarrage de l’application d’enregistrement

Dans ce laboratoire, vous créerez du code qui vous permettra de basculer la connexion des arrêts d’applications, des startups et des recomplérations dans le Visual event.

1. Ajoutez un DropDownList à default.aspx et modifiez l’ID en ddlLogAppEvents.
2. Définissez la propriété **AutoPostBack** pour le DropDownList à **vrai**.
3. Ajoutez trois éléments à la collection d’articles pour la DropDownList. Faire le **texte** pour le premier élément Sélectionner la *valeur* et la valeur -1. Rendre le **texte** et la **valeur** du deuxième élément **vrai** et le **texte** et la **valeur** du troisième élément **faux**.
4. Ajoutez une nouvelle étiquette à default.aspx. Modifier l’ID en **lblLogAppEvents**.
5. Ouvrez la vue de code-derrière pour default.aspx et ajoutez une nouvelle déclaration pour une variable de type HealthMonitoringSection comme indiqué ci-dessous:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Ajoutez le code suivant au\_code existant dans Page Init :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Double-cliquez sur la DropDownList et ajoutez le code suivant à l’événement SelectedIndexChanged :

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Parcourir default.aspx.
2. Réglez le dropdown à **False**.
3. Effacer le journal d’application dans le visualiseur de l’événement.
4. Cliquez sur le bouton pour modifier l’attribut Debug pour l’application.
5. Rafraîchir le journal d’application dans le Visualiseur de l’événement. 

    1. Y a-t-il eu des événements enregistrés?
    2. Pourquoi ou pourquoi pas?
6. Réglez le dropdown à **True.**
7. Cliquez sur le bouton pour basculer l’attribut Debug pour l’application.
8. Rafraîchir la connexion d’application au visualiseur d’événements. 

    1. Y a-t-il eu des événements enregistrés?
    2. Quelle était la raison de l’arrêt de l’application?
9. Expérimentez l’allumage et l’arrêt de l’enregistrement et regardez les modifications apportées au fichier web.config.

## <a name="more-information"></a>Informations complémentaires :

ASP.NET modèle fournisseur de 2.0 vous permet de créer vos propres fournisseurs non seulement pour l’instrumentation d’application, mais pour beaucoup d’autres utilisations aussi bien que l’adhésion, les profils, etc. Pour obtenir des informations détaillées sur la rédaction d’un fournisseur personnalisé pour enregistrer les événements de l’application à un fichier texte, visitez [ce lien](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
