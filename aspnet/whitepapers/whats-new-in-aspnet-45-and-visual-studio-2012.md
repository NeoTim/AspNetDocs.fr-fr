---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Quelles sont les nouveautés de ASP.NET 4.5 et Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET 4.5. Il décrit également les améliorations apportées pour le développement web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 6bbfb4aa7f29e4c189da4dfdca6f2113c7550b68
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57045046"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Nouveautés d’ASP.NET 4.5 et de Visual Studio 2012
====================
> Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET 4.5. Il décrit également les améliorations apportées pour le développement web dans Visual Studio 2012. Ce document a été initialement publié sur le 29 février 2012.


- [Framework et le Runtime ASP.NET Core](#_Toc318097372)

    - [Lecture de façon asynchrone et l’écriture de requêtes et réponses HTTP](#_Toc318097373)
    - [Améliorations apportées à la gestion de HttpRequest](#_Toc318097374)
    - [Vidage de façon asynchrone une réponse](#_Toc318097375)
    - [Prise en charge de *await* et *tâche*-en fonction des Modules asynchrones et des gestionnaires](#_Toc318097376)
    - [Modules HTTP asynchrones](#_Toc318097377)
    - [Gestionnaires HTTP asynchrones](#_Toc318097378)
    - [Nouvelles fonctionnalités de Validation de requête ASP.NET](#_Toc318097379)
    - [Différées de validation de la demande (« différées »)](#_Toc318097380)
    - [Prise en charge pour les demandes non validées](#_Toc318097381)
    - [Bibliothèque AntiXSS](#_Toc318097382)
    - [Prise en charge du protocole WebSockets](#_Toc318097383)
    - [Bundles et minimisation](#_Toc318097384)
    - [Améliorations des performances pour l’hébergement Web](#_Toc_perf)

        - [Facteurs de Performance clés](#_Toc_perf_1)
        - [Configuration requise pour les nouvelles fonctionnalités de performances](#_Toc_perf_2)
        - [Partage des assemblys](#_Toc_perf_3)
        - [À l’aide de la compilation JIT multicœur pour un démarrage plus rapide](#_Toc_perf_4)
        - [Paramétrage du garbage collection optimiser pour la mémoire](#_Toc_perf_5)
        - [La lecture anticipée pour les applications web](#_Toc_perf_6)
- [ASP.NET Web Forms](#_Toc318097385)

    - [Contrôles de données fortement typées](#_Toc318097386)
    - [Liaison de données](#_Toc318097387)

        - [Sélection de données](#_Toc318097388)
        - [Fournisseurs de valeurs](#_Toc318097389)
        - [Filtrage par valeurs à partir d’un contrôle](#_Toc318097390)
    - [Expressions de liaison de données encodées en HTML](#_Toc318097391)
    - [Validation non obstrusive](#_Toc318097392)
    - [Mises à jour HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Projet partage entre Visual Studio 2010 et Visual Studio 2012 Release Candidate (compatibilité des projets)](#project-compatibility)
    - [Modifications de configuration dans les modèles ASP.NET 4.5 de site Web](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Prise en charge native dans IIS 7 pour le routage ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Éditeur HTML](#_Toc318097397)

        - [Tâches actives](#_Toc318097398)
        - [Prise en charge WAI-ARIA](#_Toc318097399)
        - [Nouveaux extraits de code HTML5](#_Toc318097400)
        - [Extraire vers un contrôle utilisateur](#_Toc318097401)
        - [IntelliSense pour pépites de code dans les attributs](#_Toc318097402)
        - [Changement de nom automatique de la balise correspondante lorsque vous renommez une ouverture ou la balise de fermeture](#_Toc318097403)
        - [Génération de gestionnaire d’événements](#_Toc318097404)
        - [Mise en retrait intelligente](#_Toc318097405)
        - [Réduction automatique de la saisie semi-automatique des instructions](#_Toc318097406)
    - [Éditeur JavaScript](#_Toc318097407)

        - [Code en mode plan](#_Toc318097408)
        - [Accolades correspondantes](#_Toc318097409)
        - [Atteindre la définition](#_Toc318097410)
        - [Prise en charge de ECMAScript5](#_Toc318097411)
        - [IntelliSense de DOM](#_Toc318097412)
        - [Surcharges de signature VSDOC](#_Toc318097413)
        - [Références implicites](#_Toc318097414)
    - [Éditeur CSS](#_Toc318097415)

        - [Réduction automatique de la saisie semi-automatique des instructions](#_Toc318097416)
        - [Mise en retrait hiérarchique.](#_Toc318097417)
        - [Prise en charge des attaques CSS](#_Toc318097418)
        - [Schémas spécifiques du fournisseur (- moz-, - webkit)](#_Toc318097419)
        - [Prise en charge des commentaire et de supprimer le commentaire](#_Toc318097420)
        - [Sélecteur de couleurs](#_Toc318097421)
        - [Extraits de code](#_Toc318097422)
        - [Zones personnalisées](#_Toc318097423)
    - [Inspecteur de page](#_Toc318097424)
    - [Publication](#_Toc318097425)

        - [Profils de publication](#_Toc318097426)
        - [Précompilation de ASP.NET et de fusion](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Exclusion de responsabilité](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Framework et le Runtime ASP.NET Core

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Lecture de façon asynchrone et l’écriture de requêtes et réponses HTTP

ASP.NET 4 a introduit la capacité à lire une entité de demande HTTP comme un flux à l’aide de la *HttpRequest.GetBufferlessInputStream* (méthode). Cette méthode fournie accès en continu à l’entité de demande. Toutefois, il exécutée de façon synchrone, qui est lié un thread pendant la durée d’une demande.

ASP.NET 4.5 prend en charge la capacité à lire le flux de façon asynchrone sur une entité de demande HTTP et la possibilité de vider de manière asynchrone. ASP.NET 4.5 vous donne également la possibilité de double tampon une entité de demande HTTP qui fournit des contrôleurs de faciliter l’intégration avec les gestionnaires HTTP en aval telles que les gestionnaires de page .aspx et ASP.NET MVC.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Améliorations apportées à la gestion de HttpRequest

La référence de Stream renvoyée par ASP.NET 4.5 à partir de *HttpRequest.GetBufferlessInputStream* prend en charge les méthodes de lecture à la fois synchrones et asynchrones. Le *Stream* objet retourné à partir de *GetBufferlessInputStream* implémente désormais le BeginRead et EndRead de méthodes. Asynchrone *Stream* méthodes vous permettent de lire de façon asynchrone l’entité de requête dans des segments, alors que ASP.NET libère le thread actuel entre chaque itération d’une boucle de lecture asynchrone.

ASP.NET 4.5 a également ajouté une méthode d’accompagnement pour la lecture de l’entité de demande d’une manière mis en mémoire tampon : *HttpRequest.GetBufferedInputStream*. Cette nouvelle surcharge fonctionne comme *GetBufferlessInputStream*, prenant en charge les lectures synchrones et asynchrones. Toutefois, comme il le lit, *GetBufferedInputStream* copie également les octets de l’entité dans les mémoires tampons internes d’ASP.NET afin que les gestionnaires et les modules en aval peuvent toujours accéder à l’entité de demande. Par exemple, si certaines fenêtres en amont code dans le pipeline a déjà lu l’entité de demande à l’aide *GetBufferedInputStream*, vous pouvez toujours utiliser *HttpRequest.Form* ou *HttpRequest.Files*. Cela vous permet d’effectuer le traitement asynchrone sur une demande (par exemple, un téléchargement de fichiers volumineux à une base de données de diffusion en continu), mais les pages .aspx toujours exécution et les contrôleurs MVC ASP.NET par la suite.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Vidage de façon asynchrone une réponse

Envoi des réponses à un client HTTP peut prendre beaucoup de temps lorsque le client est éloigné ou dispose d’une connexion à faible bande passante. Normalement ASP.NET met en mémoire tampon les octets de réponse comme ils sont créés par une application. ASP.NET effectue ensuite une seule opération d’envoi des mémoires tampons à recevoir à la fin du traitement des requêtes.

Si la réponse mise en mémoire tampon est élevée (par exemple, un fichier volumineux à un client de diffusion en continu), vous devez appeler périodiquement *HttpResponse.Flush* pour envoyer la sortie en mémoire tampon au client et de conserver l’utilisation de la mémoire sous contrôle. Toutefois, étant donné que *vider* est un appel synchrone, appel de manière itérative *vider* consomme toujours un thread pour la durée des demandes de potentiellement longue.

ASP.NET 4.5 ajoute la prise en charge pour l’exécution vide de façon asynchrone à l’aide de la *BeginFlush* et *EndFlush* méthodes de la *HttpResponse* classe. À l’aide de ces méthodes, vous pouvez créer des modules et asynchrones des gestionnaires asynchrones qui envoient incrémentielle des données à un client sans monopoliser les threads du système d’exploitation. Entre les deux *BeginFlush* et *EndFlush* appels, ASP.NET libère le thread actuel. Cela réduit considérablement le nombre total de threads actifs qui sont nécessaires pour prendre en charge les téléchargements HTTP longue.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Prise en charge de *await* et *tâche* -en fonction des Modules asynchrones et des gestionnaires

Le .NET Framework 4 a introduit un concept de programmation asynchrone appelé une *tâche*. Les tâches sont représentées par le *tâche* type et les types associés dans le *System.Threading.Tasks* espace de noms. Le .NET Framework 4.5 s’appuie sur cela intègrent les améliorations du compilateur qui facilitent l’utilisation avec *tâche* simple des objets. Dans le .NET Framework 4.5, les compilateurs prennent en charge les deux nouveaux mots clés : *await* et *async*. Le *await* mot clé est un raccourci syntaxique pour indiquer un morceau de code d’attente de façon asynchrone sur un autre fragment de code. Le *async* mot clé représente un indicateur que vous pouvez utiliser pour marquer des méthodes comme méthodes asynchrones basées sur des tâches.

La combinaison de *await*, *async*et le *tâche* objet rend beaucoup plus facile à écrire du code asynchrone dans .NET 4.5. ASP.NET 4.5 prend en charge ces simplifications avec les nouvelles API qui vous permettent d’écrire des modules HTTP asynchrones et les gestionnaires HTTP asynchrones à l’aide des nouvelles améliorations du compilateur.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Modules HTTP asynchrones

Supposons que vous souhaitez effectuer un travail asynchrone dans une méthode qui retourne un *tâche* objet. L’exemple de code suivant définit une méthode asynchrone qui effectue un appel asynchrone pour télécharger la page d’accueil de Microsoft. Remarquez l’utilisation de la *async* mot clé dans la signature de méthode et la *await* l’appel à *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

C’est tout ce que vous devez écrire : le .NET Framework gère automatiquement le déroulement de la pile des appels tout en attendant la fin du téléchargement, ainsi que de restaurer automatiquement la pile des appels, une fois que le téléchargement est terminé.

Supposons maintenant que vous souhaitez utiliser cette méthode asynchrone dans un module HTTP ASP.NET asynchrone. ASP.NET 4.5 comprend une méthode d’assistance (*EventHandlerTaskAsyncHelper*) et un nouveau type de délégué (*TaskEventHandler*) que vous pouvez utiliser pour intégrer les méthodes asynchrones basées sur une tâche avec l’ancien modèle de programmation asynchrone exposée par le pipeline HTTP ASP.NET. Cet exemple montre comment :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Gestionnaires HTTP asynchrones

L’approche traditionnelle à l’écriture de gestionnaires asynchrones dans ASP.NET consiste à implémenter la *IHttpAsyncHandler* interface. ASP.NET 4.5 introduit le *HttpTaskAsyncHandler* asynchrone type de base que vous pouvez dériver à partir, ce qui le rend beaucoup plus facile d’écrire des gestionnaires asynchrones.

Le *HttpTaskAsyncHandler* type est abstrait et nécessite vous permettent de remplacer le *ProcessRequestAsync* (méthode). En interne ASP.NET prend en charge l’intégration de la signature de retournée (un *tâche* objet) de *ProcessRequestAsync* avec le modèle de programmation asynchrone antérieur utilisé par le pipeline ASP.NET.

L’exemple suivant montre comment vous pouvez utiliser *tâche* et *await* dans le cadre de l’implémentation d’un gestionnaire HTTP asynchrone :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nouvelles fonctionnalités de Validation de requête ASP.NET

Par défaut, ASP.NET exécute la validation de la demande, il examine les demandes de balisage ou de script dans les champs, en-têtes, les cookies et ainsi de suite à rechercher. Si un est détecté, ASP.NET lève une exception. Il agit comme une première ligne de défense contre les attaques de script entre sites potentiels.

ASP.NET 4.5 rend plus facile à lire les données de demande non validées de manière sélective. ASP.NET 4.5 s’intègre également la bibliothèque AntiXSS populaires, qui était auparavant une bibliothèque externe.

Les développeurs ont souvent demandé pour la possibilité de désactiver sélectivement de validation de la demande pour leurs applications. Par exemple, si votre application est un logiciel de forum, vous souhaiterez autoriser les utilisateurs à envoyer des billets de forum au format HTML et des commentaires, mais assurez-vous toujours que la validation de demande vérifie tout le reste.

ASP.NET 4.5 introduit deux fonctionnalités qui facilitent la manipuler de façon sélective les entrées non validées : différées de validation de la demande (« différées ») et l’accès aux données de la demande non validées.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Différées de validation de la demande (« différées »)

Dans ASP.NET 4.5, par défaut toutes les données de la demande est soumis à validation de demande. Toutefois, vous pouvez configurer l’application de différer la validation de la demande jusqu'à ce que vous accédez réellement les données de la demande. (Cela est parfois appelé validation des demandes différé, basée sur des termes tels que le chargement différé pour certains scénarios de données.) Vous pouvez configurer l’application pour utiliser la validation différée dans le fichier Web.config en définissant le *requestValidationMode* attribut 4.5 dans le *httpRUntime* élément, comme dans l’exemple suivant :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Lorsque le mode de validation de demande est défini vers la version 4.5, la validation de la demande est déclenchée uniquement pour une valeur de demande spécifique, et uniquement lorsque votre code accède à cette valeur. Par exemple, si votre code obtient la valeur de Request.Form["forum\_valider »], validation de demande est appelée uniquement pour cet élément dans la collection de formulaires. Aucun des autres éléments dans le *formulaire* collection sont validés. Dans les versions précédentes d’ASP.NET, validation de la demande a été déclenchée pour la collection de l’ensemble de la demande lors de l’accès de n’importe quel élément dans la collection. Le nouveau comportement rend plus facile pour les différents composants d’applications examiner les différents éléments de données de la demande sans déclencher la validation de demande sur les autres éléments.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Prise en charge pour les demandes non validées

Validation de demande différée seul ne résout pas le problème de manière sélective en contournant la validation de la demande. L’appel à Request.Form["forum\_valider »] toujours déclencheurs validation de la demande pour cette valeur de la demande spécifique. Toutefois, vous souhaiterez peut-être accéder à ce champ sans déclencher la validation, car vous souhaitez autoriser le balisage dans ce champ.

Pour cela, ASP.NET 4.5 prend désormais en charge les accès non validé pour demander des données. ASP.NET 4.5 propose une nouvelle *Unvalidated* propriété de collection dans le *HttpRequest* classe. Cette collection fournit l’accès à toutes les valeurs courantes des données de requête, comme *formulaire*, *QueryString*, *Cookies*, et *Url*.

À l’aide de l’exemple de forum, pour être en mesure de lire les données de la demande non validées, vous devez d’abord configurer l’application pour utiliser le nouveau mode de validation de demande :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Vous pouvez ensuite utiliser le *HttpRequest.Unvalidated* propriété à lire la valeur de formulaire non validées :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Sécurité - *utiliser les données de la demande non validées avec précaution !* ASP.NET 4.5 a ajouté les propriétés de la demande non validées et les collections pour le rendre plus facile d’accéder aux données de la demande non validées très spécifique. Toutefois, vous devez toujours effectuer validation personnalisée sur les données de demande brute pour vous assurer que dangereux texte n’est pas rendu dans les utilisateurs.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Bibliothèque AntiXSS

En raison de la popularité de Microsoft AntiXSS Library, ASP.NET 4.5 intègre désormais des routines d’encodage principales à partir de la version 4.0 de cette bibliothèque.

Les routines d’encodage sont implémentées par le *AntiXssEncoder* type dans le nouveau *System.Web.Security.AntiXss* espace de noms. Vous pouvez utiliser la *AntiXssEncoder* type directement en appelant une des méthodes d’encodage statiques qui sont implémentées dans le type. Toutefois, l’approche la plus simple pour l’utilisation des nouvelles routines anti-XSS consiste à configurer une application ASP.NET pour utiliser le *AntiXssEncoder* classe par défaut. Pour ce faire, ajoutez l’attribut suivant au fichier Web.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Lorsque le *encoderType* attribut est configuré pour utiliser le *AntiXssEncoder* type, tous les de sortie automatiquement le codage dans ASP.NET utilise les nouvelles routines d’encodage.

Voici les parties de la bibliothèque AntiXSS externe qui ont été intégrées dans ASP.NET 4.5 :

- *HtmlEncode*, *HtmlFormUrlEncode*, and *HtmlAttributeEncode*
- *XmlAttributeEncode* et *XmlEncode*
- *UrlEncode* et *UrlPathEncode* (nouveau)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Prise en charge du protocole WebSockets

Protocole WebSockets est un protocole réseau basé sur des normes qui définit comment établir une communication bidirectionnelle sécurisée et en temps réel entre un client et un serveur via HTTP. Microsoft a collaboré avec l’IETF et W3C des organismes de normalisation pour aider à définir le protocole. Le protocole WebSocket est pris en charge par n’importe quel client (pas seulement des navigateurs), avec Microsoft s’investissent des ressources substantielles prenant en charge le protocole WebSockets sur le client et les systèmes d’exploitation mobiles.

Protocole WebSockets rend beaucoup plus facile de créer des transferts de données longues entre un client et un serveur. Par exemple, est beaucoup plus facile à écrire une application de conversation, car vous pouvez établir une connexion de longs true entre un client et un serveur. Il est inutile de recourir à des solutions de contournement telles que l’interrogation périodique ou d’interrogation longue HTTP afin de simuler le comportement d’un socket.

ASP.NET 4.5 et IIS 8 incluent la prise en charge de WebSocket bas niveau, permettant aux développeurs ASP.NET d’utiliser des API gérées pour asynchrone lire et écrire des chaînes et des données binaires sur un objet WebSocket. Pour ASP.NET 4.5, il y a une nouvelle *System.Web.WebSockets* espace de noms qui contient des types pour l’utilisation du protocole WebSockets.

Un client de navigateur établit une connexion WebSocket en créant un DOM *WebSocket* objet qui pointe vers une URL dans une application ASP.NET, comme dans l’exemple suivant :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Vous pouvez créer des points de terminaison WebSockets dans ASP.NET à l’aide de n’importe quel type de module ou le gestionnaire. Dans l’exemple précédent, un fichier .ashx a été utilisé, étant donné que les fichiers .ashx constituent un moyen rapide pour créer un gestionnaire.

Conformément au protocole WebSocket, une application ASP.NET accepte une demande de client WebSocket en indiquant que la demande doit être mis à niveau à partir d’une requête HTTP GET à une demande de WebSockets. Voici un exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

Le *AcceptWebSocketRequest* méthode accepte un délégué de fonction, car ASP.NET se déroule de la requête HTTP actuelle et puis transfère le contrôle au délégué de fonction. Conceptuellement, cette approche est similaire à l’utilisation de *System.Threading.Thread*, où vous définissez un délégué de démarrage de thread dans quel arrière-plan travail est effectué.

Une fois ASP.NET et le client est terminé une négociation de protocole WebSocket, ASP.NET appelle votre délégué et l’application WebSockets commence à s’exécuter. L’exemple de code suivant montre une application d’écho simple qui utilise la prise en charge intégrée de WebSockets dans ASP.NET :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

La prise en charge dans .NET 4.5 pour le *await* mot clé et les opérations asynchrones basées sur la tâche est une solution naturelle pour écrire des applications de WebSockets. L’exemple de code montre qu’une demande WebSocket s’exécute complètement de façon asynchrone dans ASP.NET. L’application attend de façon asynchrone d’un message à envoyer à partir d’un client en appelant *await socket. ReceiveAsync*. De même, vous pouvez envoyer un message asynchrone à un client en appelant *await socket. SendAsync*.

Dans le navigateur, une application reçoit des messages de protocole WebSocket via un *onmessage* (fonction). Pour envoyer un message à partir d’un navigateur, vous appelez le *envoyer* méthode de la *WebSocket* type DOM, comme illustré dans cet exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

À l’avenir, nous pourrions publie des mises à jour pour cette fonctionnalité qu’abstraite disparaître certaines du codage plus bas niveau dans cette version du protocole WebSocket applications requises.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Bundles et minimisation

Regroupement vous permet de combiner des fichiers JavaScript et CSS individuels dans un regroupement qui peut être traité comme un seul fichier. Minimisation condense les fichiers JavaScript et CSS en supprimant les espaces blancs et autres caractères qui ne sont pas nécessaires. Ces fonctionnalités fonctionnent avec Web Forms, ASP.NET MVC et Web Pages.

Offres groupées sont créées à l’aide de la classe Bundle ou une de ses classes enfants, ScriptBundle et StyleBundle. Après avoir configuré une instance d’une offre groupée, l’offre groupée est accessible aux demandes entrantes en l’ajoutant simplement à une instance de BundleCollection globale. Dans les modèles par défaut, la configuration du regroupement est effectuée dans un fichier BundleConfig. Cette configuration par défaut crée des offres groupées à tous les fichiers css utilisés par les modèles et des scripts.

Offres groupées sont référencées dans les affichages à l’aide d’une des méthodes helper possible. Pour prendre en charge le rendu du balisage différents pour un regroupement lorsqu’en mode de débogage et mode de mise en production, les classes ScriptBundle et StyleBundle que la méthode d’assistance, restitue. En mode de débogage, rendu génère un balisage pour chaque ressource dans le regroupement. En mode de mise en production, rendu génère un élément de balisage unique pour le groupement intégral. Basculement entre debug et release mode est possible en modifiant l’attribut de débogage de l’élément de compilation dans le fichier web.config comme indiqué ci-dessous :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

En outre, vous pouvez définir l’activation ou la désactivation de l’optimisation directement par le biais de la propriété BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Lorsque les fichiers sont regroupés, ils sont d’abord triés par ordre alphabétique (la façon elles sont affichées dans **l’Explorateur de solutions**). Ils sont ensuite classées afin que connu des bibliothèques et leurs extensions personnalisées (telles que jQuery, MooTools et Dojo) sont chargées en premier. Par exemple, la commande finale pour le regroupement du dossier Scripts comme indiqué ci-dessus sera :

1. jQuery-1.6.2.js
2. jQuery-ui.js
3. jquery.tools.js
4. a.js

Fichiers CSS sont également triés par ordre alphabétique et puis remaniés reset.css et normalize.css précèdent n’importe quel autre fichier. Le tri finale de regroupement du dossier Styles ci-dessus sera alors :

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Améliorations des performances pour l’hébergement Web

Le .NET Framework 4.5 et Windows 8 introduisent des fonctionnalités qui peuvent vous aider à atteindre une amélioration significative des performances pour les charges de travail de serveur web. Cela inclut une réduction (jusqu'à 35 %) dans les temps de démarrage et l’encombrement de mémoire du site web qui héberge les sites qui utilisent ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Facteurs de performance clés

Dans l’idéal, tous les sites Web doivent être actifs et en mémoire afin d’assurer une réponse rapide à la demande suivante, chaque fois qu’il s’agit. Les facteurs qui peuvent affecter la réactivité de site sont les suivantes :

- Le temps que nécessaire pour un site de redémarrer après qu’un pool d’applications est recyclé. Il s’agit du temps que nécessaire pour lancer un processus de serveur web pour le site lorsque les assemblys de site ne sont plus en mémoire. (Les assemblys de plateforme sont toujours en mémoire, car ils sont utilisés par les autres sites). Cette situation est appelée « site à froid, démarrage de framework à chaud » ou simplement « à froid site démarrage ».
- Le site de la quantité de mémoire occupe. Termes du contrat pour ce sont « consommation de mémoire par site » ou « annuler le partage de jeu de travail ».

Les nouvelles améliorations de performances vous concentrer sur les deux de ces facteurs.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Configuration requise pour les nouvelles fonctionnalités de performances

La configuration requise pour les nouvelles fonctionnalités peut être décomposée ces catégories :

- Améliorations qui s’exécutent sur le .NET Framework 4.
- Améliorations qui requièrent le .NET Framework 4.5 mais qui peuvent s’exécuter sur n’importe quelle version de Windows.
- Améliorations qui sont disponibles uniquement avec le .NET Framework 4.5 s’exécutant sur Windows 8.

Les performances augmentent avec chaque niveau d’amélioration du produit que vous êtes en mesure d’activer.

Des améliorations de .NET Framework 4.5 de tirent parti des fonctionnalités de performances plus larges qui s’appliquent à d’autres scénarios.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Partage des assemblys

**Exigence**: .NET Framework 4 et Visual Studio 11 Developer Preview SDK

Des sites différents sur un serveur utilisent souvent les mêmes assemblys d’assistance (par exemple, les assemblys à partir d’une application de starter kit ou sample). Chaque site possède sa propre copie de ces assemblys dans son répertoire Bin. Même si le code de l’objet pour les assemblys est identique, elles sont des assemblys séparés physiquement, par conséquent, chaque assembly a à lire séparément lors du démarrage du site à froid et conservées séparément dans la mémoire.

La nouvelle fonctionnalité interning résout ces complications et réduit les besoins en mémoire vive et charge temps. Centralisation permet Windows conservant une seule copie de chaque assembly dans le système de fichiers et des assemblys individuels dans les dossiers d’emplacement de site sont remplacés par les liens symboliques vers la copie unique. Si une version distincte de l’assembly a besoin d’un site individuel, le lien symbolique est remplacé par la nouvelle version de l’assembly, et seulement ce site est affecté.

Partager des assemblys à l’aide des liens symboliques requiert un nouvel outil nommé aspnet\_intern.exe, qui vous permet de créer et gérer le magasin d’assemblys dans le pool interne. Il est fourni dans le cadre de Visual Studio 11 Developer Preview SDK. (Toutefois, elle fonctionnera sur un système qui contient uniquement le .NET Framework 4 installé, en supposant que vous avez installé la dernière version [mettre à jour](https://support.microsoft.com/kb/2468871).)

Pour vous assurer que tous les assemblys éligibles ont été intégrées, vous exécutez aspnet\_intern.exe périodiquement (par exemple, une fois par semaine, comme une tâche planifiée). En règle générale est la suivante :

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Pour afficher toutes les options, exécutez l’outil sans argument.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>À l’aide de la compilation JIT multicœur pour un démarrage plus rapide

**Exigence**: .NET Framework 4.5

Pour un démarrage à froid de site, non seulement les assemblys doivent-ils être lues à partir du disque, mais le site doit être compilé par JIT. Pour un site complex, cela peut ajouter des retards considérables. Une nouvelle technique à usage général dans le .NET Framework 4.5 permet de réduire ces retards en répartissant la compilation JIT sur les cœurs de processeur disponible. Pour cela, tous les éléments nécessaires et dès que possible en utilisant les informations collectées au cours de précédente lance du site. Cette fonctionnalité implémentée par le [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) (méthode).

À l’aide de plusieurs cœurs de compilation JIT est activée par défaut dans ASP.NET, vous n’avez pas besoin de rien à faire pour tirer parti de cette fonctionnalité. Si vous souhaitez désactiver cette fonctionnalité, vérifiez le paramètre suivant dans le fichier Web.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Paramétrage du garbage collection optimiser pour la mémoire

**Exigence**: .NET Framework 4.5

Une fois qu’un site est en cours d’exécution, son utilisation du tas de RÉCUPÉRATEUR de mémoire (GC) peut être un facteur significatif de sa consommation de mémoire. Comme tout RÉCUPÉRATEUR de mémoire, le garbage collector de .NET Framework rend le compromis entre temps processeur (fréquence et l’importance de collections) et la consommation de mémoire (espace supplémentaire est utilisé pour les objets nouveaux, libérés ou peut être gratuit). Pour les versions précédentes, nous avons fourni des conseils sur la façon de configurer le catalogue global pour atteindre un juste équilibre (par exemple, consultez [ASP.NET 2.0/3.5 partagé hébergeant Configuration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Pour le .NET Framework 4.5, au lieu de plusieurs paramètres autonome, un paramètre de configuration définies par la charge de travail est disponible qui offre tous les précédemment recommandée GC paramètres ainsi que les nouveau réglage qui offre des performances supplémentaires pour le site par site plage de travail.

Pour activer le réglage de la mémoire GC, ajoutez le paramètre suivant au fichier Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Si vous êtes familiarisé avec les étapes précédentes pour les modifications à aspnet.config, notez que ce paramètre remplace les anciens paramètres, par exemple, il n’est pas nécessaire de définir < gcServer, gcConcurrent, etc. Il est inutile de supprimer les anciens paramètres.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>La lecture anticipée pour les applications web

**Exigence**: .NET Framework 4.5 s’exécutant sur Windows 8

Pour les versions antérieures, Windows a inclus une technologie appelée le [prélecture](http://en.wikipedia.org/wiki/Prefetcher) afin de réduire le coût de lecture de disque de démarrage de l’application. Étant donné que le démarrage à froid est un problème principalement pour les applications clientes, cette technologie n’a pas été incluse dans Windows Server, qui inclut uniquement les composants qui sont essentielles à un serveur. La prérécupération est maintenant disponible dans la dernière version de Windows Server, où il peut optimiser le lancement des sites Web individuels.

Pour Windows Server, la prélecture n’est pas activé par défaut. Pour activer et configurer la prélecture pour l’hébergement web à haute densité, exécutez l’ensemble suivant de commandes en ligne de commande :

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Ensuite, pour intégrer la prélecture avec les applications ASP.NET, ajoutez le code suivant au fichier Web.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Contrôles de données fortement typées

Dans ASP.NET 4.5, Web Forms inclut des améliorations pour travailler avec des données. La première amélioration est les contrôles de données fortement typées. Pour les contrôles Web Forms dans les versions précédentes d’ASP.NET, vous affichez une valeur liée aux données à l’aide *Eval* et une expression de liaison de données :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Liaison de données bidirectionnelle, vous utilisez *lier*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Au moment de l’exécution, ces appels utilisent la réflexion pour lire la valeur du membre spécifié, puis afficher le résultat dans le balisage. Cette approche rend plus facile à lier aux données par rapport aux données arbitraires, unshaped.

Toutefois, telles les expressions de liaison de données ne prennent en charge les fonctionnalités comme IntelliSense pour les noms de membres, navigation (par exemple, atteindre la définition) ou la vérification de ces noms lors de la compilation.

Pour résoudre ce problème, ASP.NET 4.5 ajoute la possibilité de déclarer le type de données des données qui est lié. Procéder à l’aide de la nouvelle *ItemType* propriété. Lorsque vous définissez cette propriété, deux nouvelles variables typées sont disponibles dans l’étendue des expressions de liaison de données : *Élément* et *BindItem*. Étant donné que les variables sont fortement typées, vous obtenez le meilleur parti de l’expérience de développement Visual Studio.


Pour les expressions de liaison de données bidirectionnelles, utilisez la *BindItem* variable :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


La plupart des contrôles dans l’infrastructure ASP.NET Web Forms qui prennent en charge la liaison de données ont été mis à jour pour prendre en charge la *ItemType* propriété.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Liaison de modèle

Liaison de modèle étend la liaison de données dans les contrôles Web Forms ASP.NET pour travailler avec l’accès aux données orientés code. Il intègre des concepts à partir de la *ObjectDataSource* contrôle et à partir de la liaison de modèle dans ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Sélection de données

Pour configurer un contrôle de données pour utiliser la liaison de modèle pour sélectionner les données, vous définissez le contrôle *SelectMethod* propriété le nom d’une méthode dans le code de la page. Le contrôle de données appelle la méthode au moment opportun dans le cycle de vie de page et lie automatiquement les données retournées. Il est inutile d’appeler explicitement la *DataBind* (méthode).

Dans l’exemple suivant, le *GridView* contrôle est configuré pour utiliser une méthode nommée *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Vous créez le *GetCategories* méthode dans le code de la page. Pour une opération de sélection simple, la méthode ne requiert aucun paramètre et doit retourner un *IEnumerable* ou *IQueryable* objet. Si la nouvelle *ItemType* propriété a la valeur (qui permet fortement typées que les expressions de liaison de données, comme expliqué sous [fortement typées les contrôles de données](#_Toc318097386) précédemment), les versions de ces interfaces génériques doit être retourné, *IEnumerable&lt;T&gt;*  ou *IQueryable&lt;T&gt;*, avec le *T* paramètre correspondant au type de la *ItemType* propriété (par exemple, *IQueryable&lt;catégorie&gt;*).

L’exemple suivant montre le code pour un *GetCategories* (méthode). Cet exemple utilise le modèle Entity Framework Code First avec la base de données Northwind. Le code permet de s’assurer que la requête retourne les détails des produits connexes pour chaque catégorie par le biais de la *Include* (méthode). (Cela garantit que le *TemplateField* élément dans le balisage affiche le nombre de produits dans chaque catégorie sans nécessiter une [n + 1 Sélectionnez](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Lorsque la page s’exécute, le *GridView* appels de contrôle le *GetCategories* méthode automatiquement et affiche les données retournées en utilisant les champs configurés :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Étant donné que la méthode select retourne un *IQueryable* objet, le *GridView* contrôle peut manipuler davantage la requête avant son exécution. Par exemple, le *GridView* contrôle peut ajouter des expressions de requête pour trier et paginer à retourné *IQueryable* de l’objet avant son exécution, afin que ces opérations sont effectuées par sous-jacent Fournisseur LINQ. Dans ce cas, Entity Framework garantit que ces opérations sont effectuées dans la base de données.

L’exemple suivant montre le *GridView* contrôle modifié pour permettre le tri et de pagination :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Maintenant lorsque la page s’exécute, le contrôle peut Assurez-vous que seule la page actuelle de données s’affiche et qu’elle est triée selon la colonne sélectionnée :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Pour filtrer les données retournées, les paramètres doivent être ajoutées à la méthode select. Ces paramètres seront remplis par la liaison de modèle en cours d’exécution, et vous pouvez les utiliser pour modifier la requête avant de retourner les données.

Par exemple, supposons que vous vouliez permettre de filtrer les utilisateurs des produits en entrant un mot clé dans la chaîne de requête. Vous pouvez ajouter un paramètre à la méthode et mettre à jour le code pour utiliser la valeur du paramètre :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Ce code comprend un *où* expression si une valeur est fournie pour *mot clé* , puis retourne les résultats de requête.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Fournisseurs de valeurs

L’exemple précédent n’était pas spécifique sur l’emplacement où la valeur de la *mot clé* provenant de paramètre. Pour indiquer ces informations, vous pouvez utiliser un attribut de paramètre. Pour cet exemple, vous pouvez utiliser la *QueryStringAttribute* classe qui se trouve dans le *System.Web.ModelBinding* espace de noms :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Cela indique à la liaison de modèle pour essayer de lier une valeur de la chaîne de requête à la *mot clé* paramètre en cours d’exécution. (Cela peut impliquer la conversion de type, même si ce n’est pas dans ce cas.) Si une valeur ne peut pas être fournie et le type est non nullable, une exception est levée.

Les sources des valeurs pour ces méthodes sont appelées fournisseurs de valeurs et les attributs de paramètre qui indiquent le fournisseur de valeur à utiliser sont appelés attributs de fournisseur de valeur. Web Forms inclura les fournisseurs de valeurs et des attributs correspondants pour toutes les sources d’entrée d’utilisateur standard dans une application Web Forms, telles que la chaîne de requête, les cookies, les valeurs de formulaire, contrôles, état d’affichage, l’état de session et les propriétés de profil. Vous pouvez également écrire des fournisseurs de valeurs personnalisés.

Par défaut, le nom du paramètre est utilisé comme clé pour rechercher une valeur dans la collection de fournisseurs de valeur. Dans l’exemple, le code recherche une valeur de chaîne de requête nommée mot clé (par exemple, ~ / default.aspx?keyword=chef). Vous pouvez spécifier une clé personnalisée en passant en tant qu’argument à l’attribut de paramètre. Par exemple, pour utiliser la valeur de la variable de chaîne de requête nommée q, vous pouvez effectuer ceci :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Si cette méthode est dans le code de la page, les utilisateurs peuvent filtrer les résultats en passant un mot clé à l’aide de la chaîne de requête :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Liaison de modèle effectue de nombreuses tâches que vous auriez à coder manuellement : lecture de la valeur, recherche d’une valeur null, tente de convertir vers le type approprié, la vérification si la conversion a réussi et enfin, à l’aide de la valeur dans la requête. Liaison des résultats dans beaucoup moins de code et la possibilité de réutiliser les fonctionnalités dans votre application de modèle.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrage par valeurs à partir d’un contrôle

Supposons que vous souhaitez étendre l’exemple pour permettre à l’utilisateur de choisir une valeur de filtre dans la liste déroulante. Ajouter la liste déroulante suivante au balisage et le configurer pour obtenir ses données à partir d’une autre méthode avec le *SelectMethod* propriété :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

En général, vous devez également ajouter un *EmptyDataTemplate* élément à la *GridView* contrôler afin que le contrôle affichera un message si aucun produit correspondant n’est trouvé :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Dans le code de page, ajoutez que le nouveau sélectionner la méthode pour obtenir la liste déroulante :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Enfin, mettez à jour le *GetProducts* sélectionnez méthode d’un nouveau paramètre qui contient l’ID de la catégorie sélectionnée dans la liste déroulante :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Maintenant lorsque la page s’exécute, les utilisateurs peuvent sélectionner une catégorie dans la liste déroulante et le *GridView* contrôle est automatiquement nouveau lié pour afficher les données filtrées. Cela est possible, car la liaison de modèle suit les valeurs de paramètres pour les méthodes select et détecte si une valeur de paramètre a changé après une publication (postback). Dans ce cas, la liaison de modèle force le contrôle de données associées pour rétablir la liaison aux données.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Expressions de liaison de données encodées en HTML

Vous pouvez désormais encoder en HTML les résultats des expressions de liaison de données. Ajouter un signe deux-points ( :)) à la fin de la &lt;% # préfixe qui marque l’expression de liaison de données :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Validation non obstrusive

Vous pouvez maintenant configurer les contrôles de validation intégrée pour utiliser JavaScript non obstrusif pour la logique de validation côté client. Ainsi considérablement réduit la quantité de JavaScript rendu inline dans le balisage de page et réduit la taille globale de la page. Vous pouvez configurer le code JavaScript non obstrusif pour les contrôles de validateur dans une des manières suivantes :

- Dans le monde entier en ajoutant le paramètre suivant à la *&lt;appSettings&gt;* élément dans le fichier Web.config : 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Dans le monde entier en définissant des statiques *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* propriété *UnobtrusiveValidationMode.WebForms* (en général, dans le *Application \_Démarrer* méthode dans le fichier Global.asax).
- Individuellement pour une page en définissant la nouvelle *UnobtrusiveValidationMode* propriété de la *Page* classe *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Mises à jour HTML5

Des améliorations ont été apportées à Web Forms des contrôles de serveur pour tirer parti des nouvelles fonctionnalités d’HTML5 :

- Le *TextMode* propriété de la *zone de texte* contrôle a été mis à jour pour prendre en charge les nouveaux types d’entrées HTML5 comme *e-mail*, *datetime*, et ainsi de suite.
- Le *FileUpload* contrôler prend désormais en charge les chargements de plusieurs fichiers à partir de navigateurs qui prennent en charge cette fonctionnalité HTML5.
- Validateur contrôle désormais prise en charge lors de la validation d’entrée éléments HTML5.
- Prise en charge de nouveaux éléments HTML5 qui ont des attributs qui représentent une URL maintenant runat = « server ». Par conséquent, vous pouvez utiliser les conventions ASP.NET dans les chemins d’URL, comme le ~ opérateur pour représenter la racine de l’application (par exemple, &lt;vidéo runat = « server » src="~/myVideo.wmv » /&gt;).
- Le *UpdatePanel* contrôle a été résolu pour prendre en charge des champs d’entrée de validation HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

Version bêta d’ASP.NET MVC 4 est désormais inclus avec Visual Studio 11 Beta. ASP.NET MVC est une infrastructure pour le développement d’applications Web extrêmement testables et facile à gérer en exploitant le modèle Model-View-Controller (MVC). ASP.NET MVC 4 permet de facilement créer des applications pour le Web mobile et inclut des API Web ASP.NET, ce qui vous permet de générer des services HTTP qui peuvent atteindre n’importe quel appareil. Pour plus d’informations, consultez le [Notes de publication d’ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Pages web ASP.NET 2

Nouvelles fonctionnalités sont les suivantes :

- Modèles de site nouveau et mis à jour.
- Ajout à l’aide de la validation côté client et côté serveur le *Validation* helper.
- La possibilité d’enregistrer des scripts à l’aide d’un gestionnaire de ressources.
- L’activation de connexions à partir de Facebook et d’autres sites à l’aide d’OAuth et OpenID.
- Ajout de cartes à l’aide du *mappe* helper.
- Applications Web Pages côte à côte en cours d’exécution.
- Rendu des pages pour les appareils mobiles.

Pour plus d’informations sur ces fonctionnalités et les exemples de code de page entière, consultez [fonctionnalités principales dans les Pages Web 2 bêta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Bêta

Cette section fournit des informations sur les améliorations pour le développement web dans Visual Web Developer 11 Bêta et Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projet partage entre Visual Studio 2010 et Visual Studio 2012 Release Candidate (compatibilité des projets)

Jusqu'à ce que Visual Studio 2012 Release Candidate, ouvrez un projet existant dans une version plus récente de Visual Studio a lancé l’Assistant de Conversion. Une mise à niveau du contenu (ressources) d’une solution et projet obligés de nouveaux formats qui n’étaient pas une compatibilité descendante. Par conséquent, après la conversion vous Impossible d’ouvrir le projet dans l’ancienne version de Visual Studio.

De nombreux clients nous ont dit que cela n’était pas la bonne approche. Dans Visual Studio 11 Beta, nous prenons désormais en charge partage des projets et des solutions avec Visual Studio 2010 SP1. Cela signifie que si vous ouvrez un projet 2010 dans Visual Studio 2012 Release Candidate, vous serez toujours être en mesure d’ouvrir le projet dans Visual Studio 2010 SP1.

> [!NOTE]
> Quelques types de projets ne peuvent pas être partagés entre Visual Studio 2010 SP1 et Visual Studio 2012 Release Candidate. Citons notamment certains anciens projets (par exemple, ASP.NET MVC 2 projets) ou à des fins spéciales (par exemple, les projets d’installation).

Lorsque vous ouvrez un projet Web de Visual Studio 2010 SP1 pour la première fois dans Visual Studio 11 Beta, les propriétés suivantes sont ajoutées au fichier projet :

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation et OldToolsVersion sont utilisés par le processus qui met à niveau le fichier projet. Ils n’ont aucun impact sur l’utilisation avec le projet dans Visual Studio 2010.

VisualStudioVersion est une nouvelle propriété utilisée par MSBuild 4.5 qui indique la version de Visual Studio pour le projet actuel. Étant donné que cette propriété n’existe pas dans MSBuild 4.0 (la version de MSBuild qui utilise Visual Studio 2010 SP1), nous injecter une valeur par défaut dans le fichier projet.

La propriété VSToolsPath est utilisée pour déterminer le fichier .targets correct à importer à partir du chemin représenté par le paramètre MSBuildExtensionsPath32.

Il existe également quelques modifications associées aux éléments Import. Ces modifications sont nécessaires pour prendre en charge la compatibilité entre les deux versions de Visual Studio.

> [!NOTE]
> Si un projet est partagé entre Visual Studio 2010 SP1 et Visual Studio 11 Beta sur deux ordinateurs différents, et si le projet inclut une base de données local dans l’application\_dossier de données, vous devez vous assurer que la version de SQL Server utilisée par la base de données est installé sur les deux ordinateurs.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Modifications de configuration dans les modèles ASP.NET 4.5 de site Web

Les modifications suivantes ont été apportées à la valeur par défaut *Web.config* fichier pour le site qui sont créés à l’aide de modèles de site Web dans Visual Studio 2012 Release Candidate :

- Dans le `<httpRuntime>` élément, le `encoderType` attribut est maintenant défini par défaut à utiliser les types AntiXSS qui ont été ajoutées à ASP.NET. Pour plus d’informations, consultez [bibliothèque AntiXSS](#_Toc318097382).
- Également dans le `<httpRuntime>` élément, le `requestValidationMode` attribut est défini sur « 4.5 ». Cela signifie que, par défaut, validation de la demande est configurée pour utiliser la validation différée (« différée »). Pour plus d’informations, consultez [nouvelles fonctionnalités de Validation de demande ASP.NET](#_Toc318097379).
- Le `<modules>` élément de la `<system.webServer>` section ne contient-elle pas un `runAllManagedModulesForAllRequests` attribut. (Sa valeur par défaut est false). Cela signifie que si vous utilisez une version d’IIS 7 qui n’a pas été mis à jour vers SP1, vous rencontrez des problèmes de routage dans un nouveau site. Pour plus d’informations, consultez [prise en charge Native dans IIS 7 pour le routage ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Ces modifications n’affectent pas les applications existantes. Toutefois, ils ne peuvent représenter une différence de comportement entre les sites Web existants et nouveaux sites Web que vous créez pour ASP.NET 4.5 en utilisant les nouveaux modèles.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Prise en charge native dans IIS 7 pour le routage ASP.NET

Cela n’est pas une modification apportée à ASP.NET en tant que tel, mais une modification dans les modèles pour les nouveaux projets de site Web que vous rencontrez si vous utilisez une version d’IIS 7 qui n’a pas la mise à jour SP1 appliqué.

Dans ASP.NET, vous pouvez ajouter le paramètre de configuration suivantes pour les applications pour prendre en charge le routage :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Lorsque **runAllManagedModulesForAllRequests** est true, une URL telle que `http://mysite/myapp/home` accède à ASP.NET, même s’il existe aucune *.aspx*, *.mvc*, ou une extension similaire sur le URL.

Une mise à jour a été effectuée dans IIS 7 rend le **runAllManagedModulesForAllRequests** définissant inutiles et prend en charge en mode natif de routage ASP.NET. (Pour plus d’informations sur la mise à jour, consultez l’article du Support de Microsoft [une mise à jour est disponible que permet certains gestionnaires d’IIS 7.0 ou IIS 7.5 pour gérer les demandes dont les URL ne se termine pas par un point](https://support.microsoft.com/kb/980368).)

Si votre site Web s’exécute sur IIS 7 et si IIS a été mis à jour, vous n’avez pas besoin de définir **runAllManagedModulesForAllRequests** sur true. En fait, la valeur true est déconseillée, car il ajoute inutiles une charge de traitement à la demande. Lorsque ce paramètre est true, toutes les demandes, y compris ceux pour *.htm*, *.jpg*, et autres fichiers statiques, également passer par le pipeline de demande ASP.NET.

Si vous créez un site Web ASP.NET 4.5 à l’aide de modèles fournis dans Visual Studio 2012 RC, la configuration pour le site Web n’inclut pas le **runAllManagedModulesForAllRequests** paramètre. Cela signifie que, par défaut, le paramètre est false.

Si vous exécutez ensuite le site Web sur Windows 7 sans SP1 est installé, IIS 7 n’inclut pas la mise à jour requise. Par conséquent, le routage ne fonctionne pas et vous verrez des erreurs. Si vous avez un problème où le routage ne fonctionne pas, vous pouvez effectuer deux les éléments suivants :

- Mettez à jour Windows 7 SP1, qui ajoute la mise à jour pour IIS 7.
- Installez la mise à jour qui est décrite dans l’article du Support de Microsoft répertorié précédemment.
- Définissez **runAllManagedModulesForAllRequests** sur true dans le fichier de Web.config du site Web. Notez que cette opération ajoute une certaine charge liée aux demandes.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Éditeur HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Tâches actives

En mode conception, des propriétés complexes de contrôles de serveur souvent ont associé boîtes de dialogue et Assistants pour faciliter leur définition. Par exemple, vous pouvez utiliser une boîte de dialogue pour ajouter une source de données à un *Repeater* contrôler ou ajouter des colonnes à une *GridView* contrôle.

Toutefois, ce type de l’aide de l’interface utilisateur de propriétés complexes n’était pas disponible en mode Source. Par conséquent, Visual Studio 11 présente les tâches guidées de vue de Source. Tâches guidées sont sensibles au contexte raccourcis pour les fonctionnalités couramment utilisées dans les éditeurs de c# et Visual Basic.

Pour les contrôles Web Forms ASP.NET, tâche guidée apparaît sur les balises serveur comme un glyphe de petite lorsque le point d’insertion est à l’intérieur de l’élément :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

La tâche guidée se développe lorsque vous cliquez sur le glyphe ou appuyez sur CTRL +. (point), comme dans les éditeurs de code. Il affiche ensuite les raccourcis qui sont similaires aux tâches actives en mode Design.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Par exemple, la tâche guidée dans l’illustration précédente montre les options de tâches GridView. Si vous choisissez de modifier les colonnes, boîte de dialogue suivante s’affiche :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Remplissage dans la boîte de dialogue définit les mêmes propriétés que vous pouvez définir en mode Design. Lorsque vous cliquez sur OK, le balisage pour le contrôle est mis à jour avec les nouveaux paramètres :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Prise en charge WAI-ARIA

Il devient impératif d’écriture de sites Web accessibles. Le [accessibilité WAI-ARIA standard](http://www.w3.org/WAI/intro/aria) définit la façon dont les développeurs doivent rédiger des sites Web accessibles. Cette norme est maintenant entièrement pris en charge dans Visual Studio.

Par exemple, le *rôle* attribut a maintenant des fonctionnalités IntelliSense complètes :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

La norme WAI-ARIA introduit également les attributs qui sont précédés de *aria -* qui vous permettent d’ajouter une sémantique à un document HTML5. Visual Studio également prend entièrement en charge ces *aria -* attributs :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nouveaux extraits de code HTML5

Pour le rendre plus rapide et plus facile d’écrire le balisage de HTML5 couramment utilisé, Visual Studio inclut un nombre d’extraits de code. Par exemple, l’extrait de code vidéo :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Pour appeler l’extrait de code, appuyez sur Tab à deux reprises quand l’élément est sélectionné dans IntelliSense :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Cela produit un extrait de code que vous pouvez personnaliser.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extraire vers un contrôle utilisateur

Dans les grandes pages web, il peut être judicieux de déplacer des éléments individuels dans des contrôles utilisateur. Cette forme de refactorisation peut aider à améliorer la lisibilité de la page et peut simplifier la structure de page.

Pour simplifier ce processus, lorsque vous modifiez des pages Web Forms en mode Source, vous pouvez maintenant sélectionner du texte dans une page, faites un clic droit, puis choisissez Extraire à contrôle utilisateur :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense pour pépites de code dans les attributs

Visual Studio a toujours fourni IntelliSense pour pépites de code côté serveur dans une page ou un contrôle. Désormais, Visual Studio inclut IntelliSense pour pépites de code dans des attributs HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Cela rend plus facile de créer des expressions de liaison de données :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Changement de nom automatique de la balise correspondante lorsque vous renommez une ouverture ou la balise de fermeture

Si vous renommez un élément HTML (par exemple, vous modifiez un *div* balise à un *en-tête* balise), l’ouverture ou la balise de fermeture correspondante change également en temps réel.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Cela permet d’éviter l’erreur où vous oubliez de modifier une balise de fermeture ou de modifier une autre.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Génération de gestionnaire d’événements

Visual Studio inclut désormais les fonctionnalités en mode Source pour vous aider à écrire des gestionnaires d’événements et les lier manuellement. Si vous modifiez un nom d’événement dans la vue de Source, IntelliSense affiche &lt;créer un nouvel événement&gt;, ce qui créera un gestionnaire d’événements dans le code de la page qui a la signature appropriée :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Par défaut, le Gestionnaire d’événements utilise l’ID du contrôle pour le nom de la méthode de gestion des événements :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Le Gestionnaire d’événements résultant ressemblera à ceci (dans ce cas, en c#) :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Mise en retrait intelligente

Lorsque vous appuyez sur entrée à l’intérieur un élément HTML vide, l’éditeur placera le point d’insertion au bon endroit :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Si vous appuyez sur entrée à cet emplacement, la balise de fermeture est déplacée vers le bas et mis en retrait pour correspondre à la balise d’ouverture. Le point d’insertion est également mis en retrait :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Réduction automatique de la saisie semi-automatique des instructions

La liste IntelliSense dans Visual Studio maintenant filtres selon ce que vous tapez afin qu’il affiche les options pertinentes uniquement :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense également les filtres en fonction de la casse des mots individuels dans la liste IntelliSense. Par exemple, si vous tapez « dl », dl et asp : DataList sont affichés :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Cette fonctionnalité rend plus rapide pour obtenir la saisie semi-automatique des instructions pour les éléments connus.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>éditeur de code JavaScript

L’éditeur JavaScript dans Visual Studio 2012 Release Candidate est complètement nouveau et il améliore considérablement l’expérience de l’utilisation de JavaScript dans Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Plan du code

Régions en mode plan sont désormais automatiquement créées pour toutes les fonctions, ce qui vous permet de réduire des sections du fichier qui ne sont pas pertinentes pour votre focus actuel.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Accolades correspondantes

Lorsque vous placez le point d’insertion sur une accolade ouvrante ou fermante, l’éditeur met en surbrillance celui correspondant.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Atteindre la définition

Commande de définition permettant d’atteindre vous permet d’accéder à la source pour une fonction ou une variable.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Prise en charge de ECMAScript5

L’éditeur prend en charge les API et la nouvelle syntaxe ECMAScript5, la dernière version de la norme qui décrit le langage JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

IntelliSense pour les API DOM a été améliorée, avec prise en charge de nombreuses nouvelles API de HTML5, y compris *querySelector*, stockage DOM, la messagerie entre documents et *canevas*. DOM IntelliSense est désormais contrôlé par un seul fichier JavaScript simple, plutôt que par une définition de bibliothèque de type natif. Cela rend plus facile à étendre ou remplacer.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Surcharges de signature VSDOC

Des commentaires IntelliSense détaillés peuvent être déclarés maintenant pour les surcharges distincts des fonctions JavaScript à l’aide de la nouvelle *&lt;signature&gt;* élément, comme illustré dans cet exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Références implicites

Vous pouvez maintenant ajouter des fichiers JavaScript à une liste centrale qui est implicitement incluse dans la liste des fichiers que n’importe quel donné fichier ou un bloc de références de JavaScript, ce qui signifie que vous obtiendrez IntelliSense pour son contenu. Par exemple, vous pouvez ajouter des fichiers de jQuery à la liste centrale des fichiers, et vous obtiendrez IntelliSense pour les fonctions de jQuery dans n’importe quel bloc JavaScript du fichier, si vous l’avez explicitement référencé (à l’aide de / / / &lt;référence /&gt;) ou non.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>éditeur CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Réduction automatique de la saisie semi-automatique des instructions

La liste IntelliSense des valeurs prises en charge par le schéma sélectionné et des filtres de désormais CSS basées sur les propriétés CSS.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense prend également en charge les recherches de casse de titre :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Mise en retrait hiérarchique

L’éditeur CSS utilise mise en retrait pour afficher les règles hiérarchiques, ce qui vous donne une vue d’ensemble de la façon dont les règles en cascade sont logiquement organisés. Dans l’exemple suivant, le #list un sélecteur est un enfant en cascade de liste et n’est donc mise en retrait.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

L’exemple suivant montre l’héritage plus complexe :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

La mise en retrait d’une règle est déterminée par les règles de son parent. Mise en retrait hiérarchique est activé par défaut, mais vous pouvez le désactiver la boîte de dialogue Options (outils, Options dans la barre de menus) :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Prise en charge des attaques CSS

Analyse de plusieurs centaines de fichiers CSS réel montre qu’attaques CSS sont très courantes, et maintenant Visual Studio prend en charge les plus couramment utilisés. Cette prise en charge inclut IntelliSense et la validation de l’étoile (\*) et trait de soulignement (\_) piratages de propriété :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Attaques de sélecteur classique sont également pris en charge afin que la mise en retrait hiérarchique est maintenue même lorsque ceux-ci sont appliqués. Un hack sélecteur classique utilisé pour cibler Internet Explorer 7 consiste à faire précéder d’un sélecteur avec  *\*: first-enfant + html*. À l’aide de cette règle conserve la mise en retrait hiérarchique :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schémas spécifiques du fournisseur (- moz, - webkit-)

CSS3 introduit de nombreuses propriétés qui ont été implémentées par différents navigateurs à des moments différents. Cela forcés aux développeurs de coder pour des navigateurs spécifiques à l’aide de syntaxe spécifique au fournisseur. Ces propriétés spécifiques au navigateur sont désormais incluses dans IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Prise en charge des commentaire et de supprimer le commentaire

Vous pouvez commenter et supprimez les commentaires des règles CSS en utilisant les mêmes raccourcis que vous utilisez dans l’éditeur de code (Ctrl + K, C en commentaire et Ctrl + K, vous supprimez les commentaires).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Sélecteur de couleurs

Dans les versions précédentes de Visual Studio, IntelliSense pour les attributs de couleur est composé d’une liste déroulante de valeurs de couleur nommée. Cette liste a été remplacée par un sélecteur de couleurs complet.

Lorsque vous entrez une valeur de couleur, le sélecteur de couleurs s’affiche automatiquement et affiche une liste de couleurs utilisées précédemment, suivie d’une palette de couleurs par défaut. Vous pouvez sélectionner une couleur à l’aide de la souris ou le clavier.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

La liste peut être étendue dans un sélecteur de couleurs complète. Le sélecteur vous permet de contrôler le canal alpha en convertissant automatiquement n’importe quelle couleur dans RGBA lorsque vous déplacez le curseur Opacité :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Extraits de code

Extraits de code dans l’éditeur CSS rendent plus simple et plus rapide pour créer des styles entre navigateurs. De nombreuses propriétés CSS3 qui nécessitent des paramètres spécifiques au navigateur ont maintenant été restaurées dans les extraits de code.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Extraits de code CSS prennent en charge les scénarios avancés (tels que les requêtes de média CSS3) en tapant au symbole (@), qui affiche la liste IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Lorsque vous sélectionnez @media valeur et appuyez sur Tab, l’éditeur CSS insère l’extrait de code suivant :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Comme avec les extraits de code, vous pouvez créer vos propres extraits de code CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Zones personnalisées

Nommé des régions de code, qui sont déjà disponibles dans l’éditeur de code, sont désormais disponibles pour l’édition de CSS. Cela vous permet de facilement regrouper les blocs de style connexes.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Lorsqu’une zone est réduite, elle affiche le nom de la région :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspecteur de page

Inspecteur de page est un outil qui rend une page web (HTML, Web Forms, ASP.NET MVC ou Pages Web) dans l’IDE Visual Studio et vous permet d’examiner le code source et le résultat obtenu. Pour les pages ASP.NET, l’inspecteur de Page vous permet de déterminer quel code côté serveur a produit le balisage HTML qui est restitué dans le navigateur.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Pour plus d’informations sur l’inspecteur de Page, consultez les didacticiels suivants :

- À l’aide de l’inspecteur de Page dans [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- À l’aide de l’inspecteur de Page dans [Web Forms ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publication

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profils de publication

Dans Visual Studio 2010, les informations de publication pour les projets d’application Web ne sont pas stockées dans le contrôle de version et ne sont pas conçues pour le partage avec d’autres utilisateurs. Dans Visual Studio 2012 Release Candidate, le format du profil de publication a été modifié. Il est devenu un artefact de l’équipe, et il est désormais facile de tirer parti des générations basées sur MSBuild. Les informations de configuration de build sont dans la boîte de dialogue Publier afin que vous pouvez facilement basculer les configurations de build avant la publication.

Publier les profils sont stockés dans le dossier PublishProfiles. Dépend de l’emplacement du dossier sur le langage de programmation que vous utilisez :

- C# : Properties\PublishProfiles
- Visual Basic : Mon Project\PublishProfiles

Chaque profil est un fichier MSBuild. Lors de la publication, ce fichier est importé dans le fichier du projet MSBuild. Dans Visual Studio 2010, si vous souhaitez apporter des modifications au processus de publication ou le package, vous devez placer vos personnalisations dans un fichier nommé **nom_projet**.WPP cible. Il est toujours pris en charge, mais vous pouvez maintenant placer vos personnalisations dans le profil de publication lui-même. De cette façon, les personnalisations peuvent être utilisées uniquement pour ce profil.

Vous pouvez également tirer parti des profils à partir de MSBuild de publication. Pour ce faire, utilisez la commande suivante lorsque vous générez le projet :

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

La valeur de project.csproj est le chemin d’accès du projet, et ProfileName est le nom du profil à publier. Vous pouvez également, au lieu d’en passant le nom de profil pour le *PublishProfile* propriété, vous pouvez transmettre le chemin d’accès complet pour le profil de publication.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Précompilation de ASP.NET et de fusion

Pour les projets d’application Web, Visual Studio 2012 Release Candidate ajoute une option dans la page de propriétés Package/Publication Web qui vous permet de précompiler et fusionner le contenu de votre site lorsque vous publiez ou le projet de package. Pour afficher ces options, cliquez sur le projet dans l’Explorateur de solutions, choisissez Propriétés, puis la page de propriétés Package/Publication Web. L’illustration suivante montre le précompiler cette application avant de l’option de publication.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Lorsque cette option est sélectionnée, Visual Studio précompile l’application chaque fois que vous publiez ou le package de l’application web. Si vous souhaitez contrôler la façon dont le site est précompilé ou comment les assemblys sont fusionnés, cliquez sur le bouton Avancé pour configurer ces options.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Le serveur web par défaut pour les projets de test web dans Visual Studio est désormais IIS Express. Le serveur de développement Visual Studio est toujours une option pour le serveur web local pendant le développement, mais IIS Express est désormais le serveur recommandé. L’expérience d’utilisation d’IIS Express dans Visual Studio 11 Beta est très similaire à l’aide dans Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Exclusion de responsabilité

Ce document est une version préliminaire et peut être modifié substantiellement avant le lancement de la mise en production commerciale finale du logiciel qu’il décrit.

Les informations contenues dans ce document correspondent à la connaissance que Microsoft Corporation possède des problèmes abordés à la date de la publication. Microsoft devant répondre à des conditions de marché qui évoluent, ce document ne doit pas être considéré comme un engagement de sa part, et Microsoft ne peut pas garantir l’exactitude des informations présentées à la date de la publication.

Ce livre blanc est fourni à titre d'information uniquement. MICROSOFT NE FOURNIT AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AUX INFORMATIONS CONTENUES DANS CE DOCUMENT.

L'utilisateur est tenu d'observer la réglementation relative aux droits d'auteur applicable dans son pays. Aucune partie de ce document ne peut être reproduite, stockée ou introduite dans un système de restitution, ou transmise à quelque fin ou par quelque moyen que ce soit (électronique, mécanique, photocopie, enregistrement ou autre) sans la permission expresse et écrite de Microsoft Corporation.

Microsoft peut détenir des brevets, avoir déposé des demandes d'enregistrement de brevets ou être titulaire de marques, droits d'auteur ou autres droits de propriété intellectuelle portant sur tout ou partie des éléments qui font l'objet du présent document. Sauf stipulation expresse contraire d'un contrat de licence écrit de Microsoft, la fourniture de ce document n'a pas pour effet de vous concéder une licence sur ces brevets, marques, droits d'auteur ou autres droits de propriété intellectuelle.

Sauf indication contraire, les exemples de sociétés, les organisations, les produits, les noms de domaine, adresses de messagerie, logos, personnes, lieux et événements mentionnés sont fictif et aucune association avec n’importe quel ressemblance organisation, produit, nom de domaine, courrier électronique adresse logo, personne ou des événements est destiné, ou doit être déduite.

© 2012 Microsoft Corporation. Tous droits réservés.

Microsoft et Windows sont soit des marques déposées de Microsoft Corporation, soit des marques de Microsoft Corporation aux États-Unis d'Amérique et/ou dans d'autres pays.

Les noms des sociétés et des produits mentionnés dans le présent document peuvent être des marques de leurs propriétaires respectifs.
