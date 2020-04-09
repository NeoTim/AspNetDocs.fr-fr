---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Quoi de neuf dans ASP.NET 4.5 et Visual Studio 2012 ( Microsoft Docs
author: rick-anderson
description: Ce document décrit les nouvelles fonctionnalités et améliorations qui sont introduites dans ASP.NET 4.5. Il décrit également les améliorations apportées au développement web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80676285"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Nouveautés d’ASP.NET 4.5 et de Visual Studio 2012

> Ce document décrit les nouvelles fonctionnalités et améliorations qui sont introduites dans ASP.NET 4.5. Il décrit également les améliorations apportées au développement web dans Visual Studio 2012. Ce document a été publié le 29 février 2012.

- [ASP.NET Core Runtime and Framework](#_Toc318097372)

    - [Asynchronement lecture et rédaction de demandes et de réponses HTTP](#_Toc318097373)
    - [Améliorations à la manipulation de HttpRequest](#_Toc318097374)
    - [Asynchronement rincer une réponse](#_Toc318097375)
    - [Soutien à *l’attente* et *tâche*-Basés modules asynchrones et manutentionnaires](#_Toc318097376)
    - [Modules HTTP asynchrones](#_Toc318097377)
    - [Gestionnaires HTTP asynchrones](#_Toc318097378)
    - [Nouvelles fonctionnalités de validation des demandes de ASP.NET](#_Toc318097379)
    - [Validation différée ("paresseuse")](#_Toc318097380)
    - [Prise en charge des demandes non cirées](#_Toc318097381)
    - [Bibliothèque AntiXSS](#_Toc318097382)
    - [Prise en charge du protocole WebSockets](#_Toc318097383)
    - [Regroupement et minification](#_Toc318097384)
    - [Améliorations de performance pour l’hébergement Web](#_Toc_perf)

        - [Principaux facteurs de rendement](#_Toc_perf_1)
        - [Exigences pour les nouvelles fonctionnalités de performance](#_Toc_perf_2)
        - [Partage d’assemblées communes](#_Toc_perf_3)
        - [Utilisation de la compilation multi-Core JIT pour une startup plus rapide](#_Toc_perf_4)
        - [Réglage de la collecte des ordures pour optimiser la mémoire](#_Toc_perf_5)
        - [Préféchant pour les applications web](#_Toc_perf_6)
- [Formulaires web ASP.NET](#_Toc318097385)

    - [Contrôles de données fortement typées](#_Toc318097386)
    - [Liaison de modèle](#_Toc318097387)

        - [Sélection des données](#_Toc318097388)
        - [Fournisseurs de valeur](#_Toc318097389)
        - [Filtrage par les valeurs à partir d’un contrôle](#_Toc318097390)
    - [HTML Encodé Les expressions contraignantes des données](#_Toc318097391)
    - [Validation discrète](#_Toc318097392)
    - [Mises à jour HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [Pages web ASP.NET 2](#_Toc318097395)
- [Visual Studio 2012 Candidat de sortie](#_Toc318097396)

    - [Project Sharing Between Visual Studio 2010 and Visual Studio 2012 Release Candidate (Project Compatibility)](#project-compatibility)
    - [Modifications de configuration dans ASP.NET 4.5 Modèles de site Web](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Soutien autochtone dans l’IIS 7 pour ASP.NET Routage](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Éditeur HTML](#_Toc318097397)

        - [Tâches intelligentes](#_Toc318097398)
        - [Soutien WAI-ARIA](#_Toc318097399)
        - [Nouveaux extraits HTML5](#_Toc318097400)
        - [Extrait au contrôle de l’utilisateur](#_Toc318097401)
        - [IntelliSense pour pépites de code dans les attributs](#_Toc318097402)
        - [Renommage automatique de l’étiquette assortie lorsque vous renommez une balise d’ouverture ou de fermeture](#_Toc318097403)
        - [Génération de gestionnaire d’événements](#_Toc318097404)
        - [Indent intelligent](#_Toc318097405)
        - [Achèvement de l’instruction auto-réduction](#_Toc318097406)
    - [Éditeur JavaScript](#_Toc318097407)

        - [Énoncé de code](#_Toc318097408)
        - [Correspondance d’accolade](#_Toc318097409)
        - [Aller à la définition](#_Toc318097410)
        - [Soutien ECMAScript5](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [Surcharges de signature VSDOC](#_Toc318097413)
        - [Références implicites](#_Toc318097414)
    - [éditeur CSS](#_Toc318097415)

        - [Achèvement de l’instruction auto-réduction](#_Toc318097416)
        - [Indentation hiérarchique.](#_Toc318097417)
        - [CSS hacks support](#_Toc318097418)
        - [Schémas spécifiques aux fournisseurs (-moz-,-webkit)](#_Toc318097419)
        - [Commentaire et soutien inconvenant](#_Toc318097420)
        - [Color picker](#_Toc318097421)
        - [Extraits de code](#_Toc318097422)
        - [Régions personnalisées](#_Toc318097423)
    - [Inspecteur de page](#_Toc318097424)
    - [Édition](#_Toc318097425)

        - [Profils de publication](#_Toc318097426)
        - [ASP.NET précompatation et fusion](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Avertissement](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core Runtime and Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Asynchronement lecture et rédaction de demandes et de réponses HTTP

ASP.NET 4 a introduit la possibilité de lire une entité de demande HTTP comme un flux en utilisant la méthode *HttpRequest.GetBufferlessInputStream.* Cette méthode a permis de diffuser en continu l’entité de demande. Cependant, il a exécuté de façon synchrone, ce qui a attaché un fil pendant la durée d’une demande.

ASP.NET 4.5 prend en charge la possibilité de lire les flux asynchronement sur une entité de demande HTTP, et la capacité de rincer asynchronement. ASP.NET 4.5 vous donne également la possibilité de double-tampon une entité de demande HTTP, qui fournit une intégration plus facile avec les gestionnaires HTTP en aval tels que les gestionnaires de pages .aspx et ASP.NET contrôleurs MVC.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Améliorations à la manipulation de HttpRequest

La référence Stream retournée par ASP.NET 4.5 de *HttpRequest.GetBufferlessInputStream* prend en charge à la fois les méthodes de lecture synchrones et asynchrones. *L’objet Stream* retourné de *GetBufferlessInputStream* implémente désormais les méthodes BeginRead et EndRead. Les méthodes asynchrones *Stream* vous permettent de lire asynchronement l’entité de demande en morceaux, tandis que ASP.NET libère le fil actuel entre chaque itération d’une boucle de lecture asynchrone.

ASP.NET 4.5 a également ajouté une méthode d’accompagnement pour lire l’entité de demande d’une manière tamponné: *HttpRequest.GetBufferedInputStream*. Cette nouvelle surcharge fonctionne comme *GetBufferlessInputStream*, soutenant à la fois les lectures synchrones et asynchrones. Toutefois, comme il est écrit, *GetBufferedInputStream* copie également les octets de l’entité en ASP.NET tampons internes afin que les modules et les gestionnaires en aval puissent toujours accéder à l’entité de demande. Par exemple, si un certain code en amont dans le pipeline a déjà lu l’entité de demande en utilisant *GetBufferedInputStream*, vous pouvez toujours utiliser *HttpRequest.Form* ou *HttpRequest.Files*. Cela vous permet d’effectuer un traitement asynchrone sur une demande (par exemple, en streaming d’un grand fichier télécharger dans une base de données), mais toujours exécuter .aspx pages et MVC ASP.NET contrôleurs par la suite.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Asynchronement rincer une réponse

L’envoi de réponses à un client HTTP peut prendre beaucoup de temps lorsque le client est loin ou a une connexion à faible bande passante. Normalement, ASP.NET tamponne les octets de réponse comme ils sont créés par une application. ASP.NET effectue ensuite un seul envoi des tampons accumulés à la toute fin du traitement des demandes.

Si la réponse tampon est grande (par exemple, en streaming d’un grand fichier à un client), vous devez appeler périodiquement *HttpResponse.Flush* pour envoyer la sortie tampon au client et garder l’utilisation de la mémoire sous contrôle. Cependant, parce que *Flush* est un appel synchrone, itérativement appeler *Flush* consomme toujours un thread pour la durée de demandes potentiellement à long terme.

ASP.NET 4.5 ajoute un soutien pour effectuer des bouffées de chaleur asynchrone en utilisant les méthodes *BeginFlush* et *EndFlush* de la classe *HttpResponse.* En utilisant ces méthodes, vous pouvez créer des modules asynchrones et des gestionnaires asynchrones qui envoient progressivement des données à un client sans lier les threads du système d’exploitation. Entre les appels *BeginFlush* et *EndFlush,* ASP.NET libère le fil actuel. Cela réduit considérablement le nombre total de threads actifs qui sont nécessaires afin de prendre en charge les téléchargements HTTP de longue durée.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Soutien à *l’attente* et *à la tâche* - Modules asynchrones et manutentionnaires basés

Le cadre .NET 4 a introduit un concept de programmation asynchrone appelé *une tâche*. Les tâches sont représentées par le type *de tâche* et les types connexes dans l’espace de nom *System.Threading.Tasks.* Le cadre .NET 4.5 s’appuie sur ce point avec des améliorations compilateur qui rendent le travail avec les objets *Task* simple. Dans le cadre .NET 4.5, les compilateurs prennent en charge deux nouveaux mots clés : *l’attente* et *l’async.* Le mot clé *d’attente* est un raccourci syntaxique pour indiquer qu’un morceau de code doit attendre asynchronely sur un autre morceau de code. Le mot clé *async* représente un indice que vous pouvez utiliser pour marquer les méthodes comme des méthodes asynchrones basées sur les tâches.

La combinaison *d’attente*, *async*, et *l’objet Task,* il est beaucoup plus facile pour vous d’écrire du code asynchrone en .NET 4.5. ASP.NET 4.5 prend en charge ces simplifications avec de nouvelles API qui vous permettent d’écrire des modules HTTP asynchrones et des gestionnaires HTTP asynchrones en utilisant les nouvelles améliorations compilateur.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Modules HTTP asynchrones

Supposons que vous voulez effectuer un travail asynchrone dans une méthode qui renvoie un objet *De tâche.* L’exemple de code suivant définit une méthode asynchrone qui fait un appel asynchrone pour télécharger la page d’accueil de Microsoft. Notez l’utilisation du mot clé *async* dans la signature de la méthode et *l’appel d’attente* à *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

C’est tout ce que vous avez à écrire - le cadre .NET gérera automatiquement le dénouement de la pile d’appels en attendant le téléchargement pour terminer, ainsi que la restauration automatique de la pile d’appel après le téléchargement est fait.

Supposons maintenant que vous voulez utiliser cette méthode asynchrone dans un module asynchrone ASP.NET HTTP. ASP.NET 4.5 comprend une méthode d’aide (*EventHandlerTaskAsyncHelper*) et un nouveau type de délégué (*TaskEventHandler*) que vous pouvez utiliser pour intégrer des méthodes asynchrones basées sur des tâches avec l’ancien modèle de programmation asynchrone exposé par le pipeline ASP.NET HTTP. Cet exemple montre comment :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Gestionnaires HTTP asynchrones

L’approche traditionnelle de l’écriture des gestionnaires asynchrones dans ASP.NET est de mettre en œuvre *l’interface IHttpAsyncHandler.* ASP.NET 4.5 introduit le type de base asynchrone *httpTaskAsyncHandler* dont vous pouvez tirer, ce qui rend beaucoup plus facile d’écrire des manutentionnaires asynchrones.

Le type *HttpTaskAsyncHandler* est abstrait et vous oblige à passer outre à la méthode *ProcessRequestAsync.* En interne, ASP.NET s’occupe de l’intégration de la signature de retour (un objet *De tâche)* de *ProcessRequestAsync* avec le modèle de programmation asynchrone plus ancien utilisé par le pipeline ASP.NET.

L’exemple suivant montre comment vous pouvez utiliser *Task* et *attendre* dans le cadre de la mise en œuvre d’un gestionnaire HTTP asynchrone:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nouvelles fonctionnalités de validation des demandes de ASP.NET

Par défaut, ASP.NET effectue la validation des demandes — il examine les demandes de balisage ou de script dans les champs, les en-têtes, les cookies, etc. Si le tout est détecté, ASP.NET jette une exception. Cela agit comme une première ligne de défense contre les attaques potentielles de script cross-site.

ASP.NET 4.5 facilite la lecture sélective des données de demande nonvalidées. ASP.NET 4.5 intègre également la populaire bibliothèque AntiXSS, qui était autrefois une bibliothèque externe.

Les développeurs ont souvent demandé la possibilité d’éteindre sélectivement la validation des demandes pour leurs applications. Par exemple, si votre application est un logiciel de forum, vous pouvez permettre aux utilisateurs de soumettre des publications et des commentaires de forum formatés HTML, tout en vous assurant que la validation des demandes vérifie tout le reste.

ASP.NET 4.5 introduit deux fonctionnalités qui vous facilitent le travail sélectif avec des entrées non validées : validation de la demande différée (« paresseuse ») et accès aux données de demande non validées.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Validation différée ("paresseuse")

Dans ASP.NET 4.5, par défaut, toutes les données de demande sont soumises à la validation de la demande. Toutefois, vous pouvez configurer l’application pour reporter la validation des demandes jusqu’à ce que vous accédiez réellement aux données de demande. (C’est parfois ce qu’on appelle la validation des demandes paresseuses, basée sur des termes comme le chargement paresseux pour certains scénarios de données.) Vous pouvez configurer l’application pour utiliser la validation différée dans le fichier Web.config en définissant *l’attribut requestValidationMode* à 4,5 dans l’élément *httpRUntime,* comme dans l’exemple suivant :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Lorsque le mode de validation des demandes est réglé à 4,5, la validation de la demande n’est déclenchée que pour une valeur de demande spécifique et seulement lorsque votre code accède à cette valeur. Par exemple, si votre code obtient la valeur\_de Request.Form["forum post"], la validation de la demande n’est invoquée que pour cet élément de la collection de formulaires. Aucun des autres éléments de la collection *Form* n’est validé. Dans les versions précédentes de ASP.NET, la validation des demandes a été déclenchée pour l’ensemble de la collecte de demandes lorsque tout élément de la collection a été consulté. Le nouveau comportement permet aux différents composants d’application d’examiner plus facilement les différents éléments de données de demande sans déclencher la validation des demandes sur d’autres pièces.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Prise en charge des demandes non cirées

La validation des demandes différées à elle seule ne résout pas le problème du contournement sélectif de la validation des demandes. L’appel à Request.Form["forum\_post"] déclenche toujours la validation de la demande pour cette valeur de demande spécifique. Cependant, vous voudrez peut-être accéder à ce champ sans déclencher de validation parce que vous souhaitez autoriser la balisage dans ce domaine.

Pour ce faire, ASP.NET 4.5 prend désormais en charge l’accès non validé aux données de demande. ASP.NET 4.5 comprend une nouvelle propriété de collecte *nonvalidée* dans la classe *HttpRequest.* Cette collection donne accès à toutes les valeurs communes des données de demande, comme *Form*, *QueryString*, *Cookies*, et *Url*.

À l’aide de l’exemple du forum, pour pouvoir lire les données de demande non validées, vous devez d’abord configurer l’application pour utiliser le nouveau mode de validation de demande :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Vous pouvez ensuite utiliser la propriété *httpRequest.Unvalidated* pour lire la valeur de forme nonvalidée:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Sécurité - *Utilisez les données de demande nonvalidées avec soin!* ASP.NET 4.5 a ajouté les propriétés et les collections de demandes non autorisées pour vous permettre d’accéder plus facilement aux données de demande non validées très spécifiques. Toutefois, vous devez toujours effectuer une validation personnalisée sur les données de demande brute pour vous assurer que le texte dangereux n’est pas rendu aux utilisateurs.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Bibliothèque AntiXSS

En raison de la popularité de la bibliothèque Microsoft AntiXSS, ASP.NET 4.5 intègre maintenant des routines de codage de base à partir de la version 4.0 de cette bibliothèque.

Les routines d’encodage sont implémentées par le type *AntiXssEncoder* dans le nouvel espace nom *System.Web.Security.AntiXss.* Vous pouvez utiliser le type *AntiXssEncoder* directement en appelant l’une des méthodes d’encodage statique qui sont implémentées dans le type. Cependant, l’approche la plus simple pour l’utilisation des nouvelles routines anti-XSS est de configurer une application ASP.NET pour utiliser la classe *AntiXssEncoder* par défaut. Pour ce faire, ajoutez l’attribut suivant au fichier Web.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Lorsque *l’attribut encoderType* est configuré pour utiliser le type *AntiXssEncoder,* tout le codage de sortie dans ASP.NET utilise automatiquement les nouvelles routines de codage.

Ce sont les parties de la bibliothèque externe AntiXSS qui ont été incorporées dans ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, et *HtmlAttributeEncode*
- *XmlAttributeEncode* et *XmlEncode*
- *UrlEncode* et *UrlPathEncode* (nouveau)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Prise en charge du protocole WebSockets

Le protocole WebSockets est un protocole réseau basé sur des normes qui définit comment établir des communications bidirectionnelles sécurisées en temps réel entre un client et un serveur sur HTTP. Microsoft a travaillé avec les organismes de normalisation IETF et W3C pour aider à définir le protocole. Le protocole WebSockets est pris en charge par n’importe quel client (pas seulement les navigateurs), Microsoft investissant des ressources substantielles soutenant le protocole WebSockets sur les systèmes d’exploitation clients et mobiles.

Le protocole WebSockets facilite grandement la création de transferts de données de longue durée entre un client et un serveur. Par exemple, l’écriture d’une application de chat est beaucoup plus facile parce que vous pouvez établir une véritable connexion de longue durée entre un client et un serveur. Vous n’avez pas à recourir à des solution de contournement comme le sondage périodique ou HTTP long-polling pour simuler le comportement d’une prise.

ASP.NET 4,5 et IIS 8 comprennent le support WebSockets de bas niveau, permettant aux développeurs ASP.NET d’utiliser des API gérées pour lire et écrire asynchronement des données de chaîne et binaires sur un objet WebSockets. Pour ASP.NET 4.5, il existe un nouvel espace de nom *System.Web.WebSockets* qui contient des types pour travailler avec le protocole WebSockets.

Un client navigateur établit une connexion WebSockets en créant un objet *WebSocket* DOM qui indique une URL dans une application ASP.NET, comme dans l’exemple suivant :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Vous pouvez créer des paramètres WebSockets dans ASP.NET à l’aide de tout type de module ou de gestionnaire. Dans l’exemple précédent, un fichier .ashx a été utilisé, parce que les fichiers .ashx sont un moyen rapide de créer un gestionnaire.

Selon le protocole WebSockets, une application ASP.NET accepte la demande WebSockets d’un client en indiquant que la demande doit être mise à niveau à partir d’une demande HTTP GET à une demande WebSockets. Voici un exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

La méthode *AcceptWebSocketRequest* accepte un délégué de fonction parce que ASP.NET dénoue la demande HTTP actuelle, puis transfère le contrôle au délégué de la fonction. Conceptuellement, cette approche est similaire à la façon dont vous utilisez *System.Threading.Thread*, où vous définissez un délégué thread-start dans lequel le travail d’arrière-plan est effectué.

Après ASP.NET et le client ont réussi une poignée de main WebSockets, ASP.NET appelle votre délégué et l’application WebSockets commence à fonctionner. L’exemple de code suivant montre une application d’écho simple qui utilise le support WebSockets intégré dans ASP.NET :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

Le support en .NET 4.5 pour le mot clé *en attente* et les opérations asynchrones basées sur les tâches est un ajustement naturel pour la rédaction d’applications WebSockets. L’exemple de code montre qu’une demande WebSockets s’exécute complètement asynchronement à l’intérieur ASP.NET. L’application attend asynchronement pour un message d’être envoyé d’un client en appelant *attente prise. RecevoirAsync*. De même, vous pouvez envoyer un message asynchrone à un client en appelant *en attendant la prise. EnvoyerAsync*.

Dans le navigateur, une application reçoit des messages WebSockets via une fonction *onmessage.* Pour envoyer un message à partir d’un navigateur, vous appelez la méthode *d’envoi* du type *DOM WebSocket,* comme indiqué dans cet exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

À l’avenir, nous pourrions publier des mises à jour de cette fonctionnalité qui résument une partie du codage de bas niveau qui est nécessaire dans cette version pour les applications WebSockets.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Bundles et minimisation

Le regroupement vous permet de combiner des fichiers JavaScript et CSS individuels dans un paquet qui peut être traité comme un seul fichier. Minification condense les fichiers JavaScript et CSS en supprimant l’espace blanc et d’autres caractères qui ne sont pas requis. Ces fonctionnalités fonctionnent avec web Forms, ASP.NET MVC et Web Pages.

Les forfaits sont créés à l’aide de la classe Bundle ou d’une de ses classes d’enfants, ScriptBundle et StyleBundle. Après avoir configuré une instance d’un bundle, le bundle est mis à la disposition des demandes entrantes en l’ajoutant simplement à une instance globale BundleCollection. Dans les modèles par défaut, la configuration du bundle est effectuée dans un fichier BundleConfig. Cette configuration par défaut crée des faisceaux pour tous les scripts de base et les fichiers css utilisés par les modèles.

Les paquets sont référencés à partir de vues à l’intérieur en utilisant l’une des méthodes d’assistance possibles. Afin de prendre en charge le rendu de différentes balisage pour un paquet lors du mode de débbug vs version, les classes ScriptBundle et StyleBundle ont la méthode d’aide, Render. Lorsqu’il est en mode débogé, Render générera une majoration pour chaque ressource du paquet. En mode version, Render générera un seul élément de balisage pour l’ensemble du bundle. Le basculage entre le débogé et le mode de libération peut être accompli en modifiant l’attribut de débog de l’élément de compilation dans web.config comme indiqué ci-dessous :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

En outre, l’optimisation permettant ou invalidante peut être définie directement via la propriété BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Lorsque les fichiers sont groupés, ils sont d’abord triés par ordre alphabétique (la façon dont ils sont affichés dans **Solution Explorer**). Ils sont ensuite organisés de sorte que les bibliothèques connues et leurs extensions personnalisées (telles que jQuery, MooTools, et Dojo) sont chargées en premier. Par exemple, l’ordre final pour le regroupement du dossier Scripts tel qu’indiqué ci-dessus sera :

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js (en anglais)
4. a.js a.js

Les fichiers CSS sont également triés par ordre alphabétique, puis réorganisés de sorte que reset.css et normalize.css viennent avant tout autre fichier. Le tri final du regroupement du dossier Styles indiqué ci-dessus sera le suivante:

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Améliorations de performance pour l’hébergement Web

Le cadre .NET 4.5 et Windows 8 introduisent des fonctionnalités qui peuvent vous aider à atteindre un coup de pouce significatif des performances pour les charges de travail des serveurs Web. Cela comprend une réduction (jusqu’à 35 %) dans le temps de démarrage et dans l’empreinte mémoire des sites d’hébergement web qui utilisent ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Principaux facteurs de rendement

Idéalement, tous les sites Web devraient être actifs et en mémoire pour assurer une réponse rapide à la prochaine demande, chaque fois qu’elle vient. Les facteurs qui peuvent influer sur la réactivité du site sont les suivants :

- Le temps qu’il faut à un site pour redémarrer après qu’une piscine d’applications se recycle. C’est le temps qu’il faut pour lancer un processus de serveur web pour le site lorsque les assemblages du site ne sont plus en mémoire. (Les assemblages de plate-forme sont encore en mémoire, car ils sont utilisés par d’autres sites.) Cette situation est appelée « site froid, démarrage de cadre chaud » ou simplement « démarrage de site froid ».
- Quelle mémoire le site occupe. Les termes pour cela sont «consommation de mémoire par site» ou «ensemble de travail non partagé."

Les nouvelles améliorations du rendement mettent l’accent sur ces deux facteurs.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Exigences pour les nouvelles fonctionnalités de performance

Les exigences pour les nouvelles fonctionnalités peuvent être ventilées en ces catégories :

- Améliorations qui s’gèrent sur le cadre .NET 4.
- Améliorations qui nécessitent le cadre .NET 4.5 mais peuvent s’exécuter sur n’importe quelle version de Windows.
- Améliorations qui ne sont disponibles qu’avec .NET Framework 4.5 fonctionnant sur Windows 8.

Les performances augmentent à chaque niveau d’amélioration que vous êtes en mesure d’activer.

Certaines des améliorations du cadre .NET 4.5 tirent parti des caractéristiques de rendement plus larges qui s’appliquent également à d’autres scénarios.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Partage d’assemblées communes

**Exigence**: .NET Framework 4 et Visual Studio 11 Developer Preview SDK

Différents sites sur un serveur utilisent souvent les mêmes assemblages d’assistance (par exemple, les assemblages d’un kit de démarrage ou d’une application d’échantillon). Chaque site a sa propre copie de ces assemblages dans son répertoire Bin. Même si le code objet pour les assemblages est identique, ce sont des assemblages physiquement séparés, de sorte que chaque assemblage doit être lu séparément pendant le démarrage du site froid et conservé séparément dans la mémoire.

La nouvelle fonctionnalité d’internement résout cette inefficacité et réduit à la fois les exigences de RAM et le temps de chargement. L’internement permet à Windows de conserver une seule copie de chaque assemblage dans le système de fichiers, et les assemblages individuels dans les dossiers Bin site sont remplacés par des liens symboliques vers la copie unique. Si un site individuel a besoin d’une version distincte de l’assemblage, le lien symbolique est remplacé par la nouvelle version de l’assemblage, et seul ce site est affecté.

Partager des assemblages à l’aide de\_liens symboliques nécessite un nouvel outil nommé aspnet intern.exe, qui vous permet de créer et de gérer le magasin d’assemblages internés. Il est fourni dans le cadre du Visual Studio 11 Developer Preview SDK. (Toutefois, il fonctionnera sur un système qui a seulement le cadre .NET 4 installé, en supposant que vous avez installé la dernière [mise à jour](https://support.microsoft.com/kb/2468871).)

Pour vous assurer que toutes les assemblées éligibles\_ont été internées, vous exécutez aspnet intern.exe périodiquement (par exemple, une fois par semaine comme tâche prévue). Une utilisation typique est la suivante:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Pour voir toutes les options, exécutez l’outil sans arguments.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Utilisation de la compilation multi-Core JIT pour une startup plus rapide

**Exigence**: .NET Framework 4.5

Pour une start-up de site froid, non seulement les assemblages doivent être lus à partir du disque, mais le site doit être compilé JIT. Pour un site complexe, cela peut ajouter des retards importants. Une nouvelle technique polyvalente dans le cadre .NET 4.5 réduit ces retards en répartissant la compilation JIT sur les cœurs de processeur disponibles. Il le fait autant et le plus tôt possible en utilisant les informations recueillies lors des lancements précédents du site. Cette fonctionnalité implémentée par la méthode [System.Runtime.ProfileOptimization.StartProfile.](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)

La compilation JIT à l’aide de plusieurs cœurs est activée par défaut dans ASP.NET, de sorte que vous n’avez pas besoin de faire quoi que ce soit pour profiter de cette fonctionnalité. Si vous souhaitez désactiver cette fonctionnalité, effectuez le paramètre suivant dans le fichier Web.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Réglage de la collecte des ordures pour optimiser la mémoire

**Exigence**: .NET Framework 4.5

Une fois qu’un site est en cours d’exécution, son utilisation du tas de collecteurs d’ordures (GC) peut être un facteur important dans sa consommation de mémoire. Comme tout éboueur, le .NET Framework GC effectue des compromis entre le temps du processeur (fréquence et signification des collections) et la consommation de mémoire (espace supplémentaire utilisé pour les objets nouveaux, libérés ou libres). Pour les versions précédentes, nous avons fourni des conseils sur la façon de configurer le GC pour atteindre le bon équilibre (par exemple, voir [ASP.NET 2.0/3.5 Configuration d’hébergement partagée](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Pour le cadre .NET 4.5, au lieu de plusieurs paramètres autonomes, un paramètre de configuration défini par la charge de travail est disponible qui permet tous les paramètres GC précédemment recommandés ainsi que de nouveaux réglages qui offrent des performances supplémentaires pour l’ensemble de travail par site.

Pour activer l’accord de mémoire GC, ajoutez le paramètre suivant au fichier Windows-Microsoft.NET-Framework-v4.0.30319-aspnet.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Si vous connaissez les directives précédentes pour les modifications à aspnet.config, notez que ce paramètre remplace les anciens paramètres - par exemple, il n’est pas nécessaire de définir gcServer, gcConcurrent, etc. Vous n’avez pas à supprimer les anciens paramètres.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Préféchant pour les applications web

**Exigence**: .NET Framework 4.5 fonctionnant sur Windows 8

Pour plusieurs versions, Windows a inclus une technologie connue sous le nom [de prefetcher](http://en.wikipedia.org/wiki/Prefetcher) qui réduit le coût de lecture du disque de démarrage d’application. Parce que le démarrage à froid est un problème principalement pour les applications client, cette technologie n’a pas été incluse dans Windows Server, qui comprend uniquement les composants qui sont essentiels à un serveur. Prefetching est maintenant disponible dans la dernière version de Windows Server, où il peut optimiser le lancement de sites Web individuels.

Pour Windows Server, le prefetcher n’est pas activé par défaut. Pour activer et configurer le prefetcher pour l’hébergement Web à haute densité, exécutez l’ensemble de commandes suivant à la ligne de commande :

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Ensuite, pour intégrer le prefetcher avec ASP.NET applications, ajoutez ce qui suit au fichier Web.config :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Formulaires web ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Contrôles de données fortement typées

Dans ASP.NET 4.5, les formulaires Web comprennent certaines améliorations pour travailler avec les données. La première amélioration est fortement tapée contrôles de données. Pour les formulaires Web dans les versions précédentes de ASP.NET, vous affichez une valeur liée aux données à l’aide *d’Eval* et d’une expression contraignante de données :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Pour la liaison de données bidirectionnel, vous utilisez *Bind*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Au moment de l’exécution, ces appels utilisent la réflexion pour lire la valeur du membre spécifié, puis afficher le résultat dans la balisage. Cette approche facilite la liaison des données par rapport aux données arbitraires et non façonnées.

Cependant, les expressions de liaison de données comme celle-ci ne prennent pas en charge des fonctionnalités comme IntelliSense pour les noms des membres, la navigation (comme Go To Definition), ou la vérification du temps de compilation de ces noms.

Pour résoudre ce problème, ASP.NET 4.5 ajoute la possibilité de déclarer le type de données des données auxquelles un contrôle est lié. Vous le faites en utilisant la nouvelle propriété *ItemType.* Lorsque vous définissez cette propriété, deux nouvelles variables typées sont disponibles dans la portée des expressions de liaison de données : *Item* et *BindItem*. Parce que les variables sont fortement typées, vous obtenez tous les avantages de l’expérience de développement Visual Studio.

Pour les expressions bidirectionnels de liaison de données, utilisez la variable *BindItem* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

La plupart des contrôles dans le cadre ASP.NET web Forms qui prennent en charge la liaison des données ont été mis à jour pour prendre en charge la propriété *ItemType.*

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Liaison de modèle

La liaison de modèle étend la liaison de données dans ASP.NET les contrôles web de formulaires pour travailler avec l’accès aux données axée sur le code. Il intègre des concepts du contrôle *ObjectDataSource* et de la liaison de modèle dans ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Sélection des données

Pour configurer un contrôle de données pour utiliser la liaison du modèle pour sélectionner les données, vous définissez la propriété *SelectMethod* du contrôle au nom d’une méthode dans le code de la page. Le contrôle des données appelle la méthode au moment approprié dans le cycle de vie de la page et lie automatiquement les données retournées. Il n’est pas nécessaire d’appeler explicitement la méthode *DataBind.*

Dans l’exemple suivant, le contrôle *GridView* est configuré pour utiliser une méthode nommée *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Vous créez la méthode *GetCategories* dans le code de la page. Pour une opération simple, la méthode n’a pas besoin de paramètres et doit retourner un objet *IEnumerable* ou *IQueryable.* Si la nouvelle propriété *ItemType* est définie (ce qui permet des expressions fortement typées de liaison de données, comme expliqué dans les contrôles de [données fortement typés](#_Toc318097386) plus tôt), les versions génériques de ces interfaces doivent être retournées — *&lt;IEnumerable T&gt; * ou *IQueryable&lt;T&gt;*, avec le paramètre *T* correspondant au type de propriété *ItemType* (par exemple, *catégorie&lt;&gt;IQueryable*).

L’exemple suivant montre le code d’une méthode *GetCategories.* Cet exemple utilise le modèle Entity Framework Code First avec la base de données de l’échantillon Northwind. Le code s’assure que la requête renvoie les détails des produits connexes pour chaque catégorie par le moyen de la méthode *Inclure.* (Cela garantit que l’élément *TemplateField* dans la balisage affiche le nombre de produits dans chaque catégorie sans exiger une [sélection n '1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Lorsque la page s’exécute, le contrôle *GridView* appelle automatiquement la méthode *GetCategories* et rend les données retournées à l’aide des champs configurés :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Étant donné que la méthode sélectionnée renvoie un objet *IQueryable,* le contrôle *GridView* peut manipuler davantage la requête avant de l’exécuter. Par exemple, le contrôle *GridView* peut ajouter des expressions de requête pour le tri et la paging à l’objet *IQueryable* retourné avant qu’il ne soit exécuté, de sorte que ces opérations sont effectuées par le fournisseur sous-jacent LINQ. Dans ce cas, le Cadre d’entités s’assurera que ces opérations sont effectuées dans la base de données.

L’exemple suivant montre le contrôle *GridView* modifié pour permettre le tri et le pagination :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Maintenant, lorsque la page s’exécute, le contrôle peut s’assurer que seule la page actuelle de données est affichée et qu’elle est commandée par la colonne sélectionnée :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Pour filtrer les données retournées, les paramètres doivent être ajoutés à la méthode de sélection. Ces paramètres seront peuplés par la liaison du modèle au moment de l’exécution, et vous pouvez les utiliser pour modifier la requête avant de retourner les données.

Supposons, par exemple, que vous souhaitez laisser filtrer les produits par les utilisateurs en entrant un mot clé dans la chaîne de requête. Vous pouvez ajouter un paramètre à la méthode et mettre à jour le code pour utiliser la valeur du paramètre :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Ce code inclut une *expression où* si une valeur est fournie pour *le mot clé,* puis renvoie les résultats de la requête.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Fournisseurs de valeur

L’exemple précédent n’était pas précis sur l’endroit d’où venait la valeur du paramètre du *mot clé.* Pour indiquer ces informations, vous pouvez utiliser un attribut de paramètre. Pour cet exemple, vous pouvez utiliser la classe *QueryStringAttribute* qui est dans l’espace nom *System.Web.ModelBinding:*

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Cela indique la liaison du modèle pour essayer de lier une valeur de la chaîne de requête au paramètre *de mot clé* au moment de l’exécution. (Cela peut impliquer l’exécution de conversion de type, bien qu’il ne le fait pas dans ce cas.) Si une valeur ne peut pas être fournie et que le type n’est pas annulable, une exception est prévue.

Les sources de valeurs de ces méthodes sont appelées fournisseurs de valeur, et les attributs de paramètres qui indiquent les attributs de valeur que le fournisseur à utiliser sont appelés attributs de fournisseur de valeur. Les formulaires Web comprendront des fournisseurs de valeur et des attributs correspondants pour toutes les sources typiques d’entrée de l’utilisateur dans une application Web Forms, telles que la chaîne de requêtes, les cookies, les valeurs de formulaire, les contrôles, l’état de vue, l’état de session et les propriétés de profil. Vous pouvez également écrire des fournisseurs de valeur personnalisés.

Par défaut, le nom du paramètre est utilisé comme clé pour trouver une valeur dans la collection de fournisseur de valeur. Dans l’exemple, le code cherchera une valeur de requête-chaîne nommée mot-clé (par exemple, '/default.aspx?keyword’chef). Vous pouvez spécifier une clé personnalisée en la passant comme argument à l’attribut de paramètre. Par exemple, pour utiliser la valeur de la variable de corde de requête nommée q, vous pouvez le faire :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Si cette méthode est dans le code de la page, les utilisateurs peuvent filtrer les résultats en passant un mot clé à l’aide de la chaîne de requête:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

La liaison de modèle accomplit de nombreuses tâches que vous auriez autrement à coder à la main: la lecture de la valeur, la vérification d’une valeur nulle, en essayant de le convertir au type approprié, vérifier si la conversion a été réussie, et enfin, en utilisant la valeur dans la requête. La liaison de modèle entraîne beaucoup moins de code et dans la possibilité de réutiliser la fonctionnalité tout au long de votre application.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrage par les valeurs à partir d’un contrôle

Supposons que vous souhaitez donner l’exemple pour permettre à l’utilisateur de choisir une valeur de filtre à partir d’une liste d’abandon. Ajoutez la liste d’abandon suivante à la balisage et configurez-la pour obtenir ses données d’une autre méthode en utilisant la propriété *SelectMethod* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

En règle générale, vous ajoutez également un élément *EmptyDataTemplate* au contrôle *GridView* afin que le contrôle affiche un message si aucun produit correspondant n’est trouvé :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Dans le code de page, ajoutez la nouvelle méthode de sélection pour la liste d’abandon :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Enfin, mettez à jour la méthode de sélection *Des GetProducts* pour prendre un nouveau paramètre qui contient l’ID de la catégorie sélectionnée à partir de la liste d’abandon :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Maintenant, lorsque la page s’exécute, les utilisateurs peuvent sélectionner une catégorie à partir de la liste d’abandon, et le contrôle *GridView* est automatiquement re-lié pour afficher les données filtrées. Cela est possible parce que la liaison du modèle suit les valeurs des paramètres pour certaines méthodes et détecte si la valeur des paramètres a changé après un postback. Dans l’affirmative, la liaison de modèle oblige le contrôle des données associé à se re-lié aux données.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML Encodé Les expressions contraignantes des données

Vous pouvez maintenant coder HTML le résultat d’expressions de liaison de données. Ajouter un côlon (:) jusqu’à la &lt;fin du préfixe de % qui marque l’expression de liaison de données :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Validation discrète

Vous pouvez maintenant configurer les commandes intégrées de validateur pour utiliser JavaScript discret pour la logique de validation côté client. Cela réduit considérablement la quantité de JavaScript rendu en ligne dans le balisage de la page et réduit la taille globale de la page. Vous pouvez configurer JavaScript discret pour les contrôles de validateur de toutes ces façons :

- Globalement en ajoutant le paramètre suivant à * &lt;l’élément&gt; appSettings* dans le fichier Web.config : 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalement en définissant la propriété static *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* à *UnobtrusiveValidationMode.WebForms* (généralement dans la méthode *De démarrage d’application\_* dans le fichier Global.asax).
- Individuellement pour une page en définissant la nouvelle propriété *UnobtrusiveValidationMode* de la classe *De page* à *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Mises à jour HTML5

Certaines améliorations ont été apportées aux contrôles du serveur Web Forms pour tirer parti des nouvelles fonctionnalités de HTML5 :

- La propriété *TextMode* du contrôle *TextBox* a été mise à jour pour prendre en charge les nouveaux types d’entrées HTML5 comme *le courrier électronique,* *l’heure de la date,* et ainsi de suite.
- Le contrôle *FileUpload* prend désormais en charge plusieurs téléchargements de fichiers à partir de navigateurs qui prennent en charge cette fonctionnalité HTML5.
- Les contrôles de validation prennent désormais en charge la validation des éléments d’entrée HTML5.
- Les nouveaux éléments HTML5 qui ont des attributs qui représentent une URL prennent désormais en charge runat "serveur". En conséquence, vous pouvez utiliser ASP.NET conventions dans les chemins d’URL, comme l’opérateur pour représenter la racine de l’application (par exemple, &lt;runat vidéo "serveur" src "/myVideo.wmv" /&gt;).
- Le contrôle *UpdatePanel* a été fixé pour prendre en charge l’affichage des champs d’entrée HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta est maintenant inclus avec Visual Studio 11 Beta. ASP.NET MVC est un cadre pour développer des applications Web hautement testables et maintenables en tirant parti du modèle Model-View-Controller (MVC). ASP.NET MVC 4 facilite la construction d’applications pour le Web mobile et comprend ASP.NET’API Web, qui vous aide à créer des services HTTP qui peuvent atteindre n’importe quel appareil. Pour plus d’informations, voir le [ASP.NET MVC 4 Communiqué Notes](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Pages web ASP.NET 2

Les nouvelles fonctionnalités incluent ce qui suit :

- Modèles de site nouveaux et mis à jour.
- Ajout d’une validation côté serveur et côté client à l’aide de l’aide *validation.*
- La possibilité d’enregistrer des scripts à l’aide d’un gestionnaire d’actifs.
- Permettre les connexions de Facebook et d’autres sites en utilisant OAuth et OpenID.
- Ajout de cartes à l’aide de l’aide *Maps.*
- Exécution des applications Pages Web côte à côte.
- Pages de rendu pour les appareils mobiles.

Pour plus d’informations sur ces fonctionnalités et des exemples de code pleine page, voir [The Top Features in Web Pages 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Développeur Web visuel 11 Bêta

Cette section fournit des informations sur les améliorations apportées au développement Web dans Visual Web Developer 11 Beta and Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Project Sharing Between Visual Studio 2010 and Visual Studio 2012 Release Candidate (Project Compatibility)

Jusqu’à Visual Studio 2012 Release Candidate, l’ouverture d’un projet existant dans une nouvelle version de Visual Studio a lancé le Conversion Wizard. Cela a forcé la mise à niveau du contenu (actifs) d’un projet et d’une solution à de nouveaux formats qui n’étaient pas compatibles avec l’arrière. Par conséquent, après la conversion, vous ne pouviez pas ouvrir le projet dans l’ancienne version de Visual Studio.

De nombreux clients nous ont dit que ce n’était pas la bonne approche. Dans Visual Studio 11 Beta, nous prenons désormais en charge le partage de projets et de solutions avec Visual Studio 2010 SP1. Cela signifie que si vous ouvrez un projet 2010 dans Visual Studio 2012 Release Candidate, vous serez toujours en mesure d’ouvrir le projet dans Visual Studio 2010 SP1.

> [!NOTE]
> Quelques types de projets ne peuvent pas être partagés entre Visual Studio 2010 SP1 et Visual Studio 2012 Candidat à la sortie. Il s’agit notamment de certains projets plus anciens (tels que ASP.NET projets MVC 2) ou de projets à des fins spéciales (comme les projets d’installation).

Lorsque vous ouvrez pour la première fois un projet Web SP1 de Visual Studio 2010 dans Visual Studio 11 Beta, les propriétés suivantes sont ajoutées au dossier du projet :

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath (en)

FileUpgradeFlags, UpgradeBackupLocation et OldToolsVersion sont utilisés par le processus qui met à niveau le fichier du projet. Ils n’ont aucun impact sur le travail avec le projet dans Visual Studio 2010.

VisualStudioVersion est une nouvelle propriété utilisée par MSBuild 4.5 qui indique la version de Visual Studio pour le projet en cours. Parce que cette propriété n’existait pas dans MSBuild 4.0 (la version de MSBuild celui Visual Studio 2010 SP1 utilise), nous injectons une valeur par défaut dans le fichier de projet.

La propriété VSToolsPath est utilisée pour déterminer le fichier .targets correct à importer du chemin représenté par le réglage MSBuildExtensionsPath32.

Il y a aussi quelques changements liés aux éléments d’importation. Ces modifications sont nécessaires afin de soutenir la compatibilité entre les deux versions de Visual Studio.

> [!NOTE]
> Si un projet est partagé entre Visual Studio 2010 SP1 et Visual Studio 11 Beta sur\_deux ordinateurs différents, et si le projet inclut une base de données locale dans le dossier App Data, vous devez vous assurer que la version de SQL Server utilisée par la base de données est installée sur les deux ordinateurs.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Modifications de configuration dans ASP.NET 4.5 Modèles de site Web

Les modifications suivantes ont été apportées au fichier *Web.config* par défaut pour le site qui sont créés à l’aide de modèles de site Web dans Visual Studio 2012 Release Candidate:

- Dans `<httpRuntime>` l’élément, l’attribut `encoderType` est maintenant défini par défaut pour utiliser les types AntiXSS qui ont été ajoutés à ASP.NET. Pour plus de détails, voir [AntiXSS Library](#_Toc318097382).
- Toujours dans `<httpRuntime>` l’élément, l’attribut `requestValidationMode` est réglé à "4.5". Cela signifie que, par défaut, la validation de la demande est configurée pour utiliser la validation différée (« paresseuse »). Pour plus de détails, consultez [les fonctionnalités de validation des demandes de ASP.NET](#_Toc318097379).
- L’élément `<modules>` `<system.webServer>` de l’article `runAllManagedModulesForAllRequests` ne contient pas d’attribut. (Sa valeur par défaut est fausse.) Cela signifie que si vous utilisez une version de l’IIS 7 qui n’a pas été mise à jour sur SP1, vous pourriez avoir des problèmes avec le routage dans un nouveau site. Pour plus d’informations, voir [Soutien autochtone dans IIS 7 pour ASP.NET Routing](#Native_Support_In_IIS7_For_ASPNET_Routine).

Ces modifications n’affectent pas les applications existantes. Cependant, ils pourraient représenter une différence de comportement entre les sites Web existants et les nouveaux sites Web que vous créez pour ASP.NET 4.5 en utilisant les nouveaux modèles.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Soutien autochtone dans l’IIS 7 pour ASP.NET Routage

Il ne s’agit pas d’un changement à ASP.NET en tant que telle, mais un changement de modèles pour les nouveaux projets de site Web qui peuvent vous affecter si vous travaillez une version de l’IIS 7 qui n’a pas eu la mise à jour SP1 appliquée.

Dans ASP.NET, vous pouvez ajouter le paramètre de configuration suivant aux applications afin de prendre en charge le routage :

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Lorsque **runAllManagedModulesForAllRequests** est vrai, `http://mysite/myapp/home` une URL comme va à ASP.NET, même s’il n’y a pas *.aspx*, *.mvc*, ou extension similaire sur l’URL.

Une mise à jour qui a été faite à l’IIS 7 rend le **runAllManagedModulesForAllRequests** réglage inutile et prend en charge ASP.NET routage natif. (Pour plus d’informations sur la mise à jour, voir l’article Microsoft Support [Une mise à jour est disponible qui permet à certains gestionnaires IIS 7.0 ou IIS 7.5 de traiter les demandes dont les URL ne se terminent pas par une période](https://support.microsoft.com/kb/980368).)

Si votre site fonctionne sur l’IIS 7 et si IIS a été mis à jour, vous n’avez pas besoin de définir **runAllManagedModulesForAllRequests** à vrai. En fait, il n’est pas recommandé de le mettre en place, car il ajoute des frais généraux de traitement inutiles à la demande. Lorsque ce paramètre est vrai, toutes les demandes, y compris celles de *.htm*, *.jpg*, et d’autres fichiers statiques, passent également par le pipeline de demande ASP.NET.

Si vous créez un nouveau site Web ASP.NET 4.5 à l’aide des modèles fournis dans Visual Studio 2012 RC, la configuration du site Web n’inclut pas le paramètre **runAllManagedModulesForAllRequests.** Cela signifie que par défaut le paramètre est faux.

Si vous exécutez ensuite le site Web sur Windows 7 sans SP1 installé, IIS 7 n’inclura pas la mise à jour requise. Par conséquent, le routage ne fonctionnera pas et vous verrez des erreurs. Si vous avez un problème où le routage ne fonctionne pas, vous pouvez faire soit ce qui suit:

- Mettez à jour Windows 7 sur SP1, qui ajoutera la mise à jour à l’IIS 7.
- Installez la mise à jour décrite dans l’article Microsoft Support énuméré précédemment.
- Définir **runAllManagedModulesForAllRequests** à vrai dans le fichier Web.config de ce site. Notez que cela ajoutera quelques frais généraux aux demandes.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Éditeur HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Tâches intelligentes

Dans la vue design, les propriétés complexes des contrôles de serveur ont souvent associés boîtes de dialogue et les assistants pour le rendre facile à les définir. Par exemple, vous pouvez utiliser une boîte de dialogue spéciale pour ajouter une source de données à un contrôle *Répéteur* ou ajouter des colonnes à un contrôle *GridView.*

Cependant, ce type d’aide d’assurance-chômage pour les propriétés complexes n’a pas été disponible dans Source view. Par conséquent, Visual Studio 11 introduit Smart Tasks for Source view. Les tâches intelligentes sont des raccourcis contextuelles pour les fonctionnalités couramment utilisées dans les éditeurs de C et Visual Basic.

Pour ASP.NET les contrôles Web Forms, les tâches intelligentes apparaissent sur les balises du serveur sous forme de petit glyphe lorsque le point d’insertion se trouve à l’intérieur de l’élément :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

La tâche intelligente s’élargit lorsque vous cliquez sur le glyph ou appuyez sur CTRL. (point), tout comme dans les éditeurs de code. Il affiche ensuite des raccourcis qui sont similaires aux tâches intelligentes dans la vue design.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Par exemple, la tâche intelligente dans l’illustration précédente montre les options GridView Tasks. Si vous choisissez Edit Columns, la boîte de dialogue suivante s’affiche :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Remplir la boîte de dialogue définit les mêmes propriétés que vous pouvez définir dans la vue Design. Lorsque vous cliquez sur OK, le balisage pour le contrôle est mis à jour avec les nouveaux paramètres:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Soutien WAI-ARIA

L’écriture de sites Web accessibles devient de plus en plus importante. La [norme d’accessibilité WAI-ARIA](http://www.w3.org/WAI/intro/aria) définit la façon dont les développeurs devraient écrire des sites Web accessibles. Cette norme est maintenant entièrement pris en charge dans Visual Studio.

Par exemple, l’attribut de *rôle* a maintenant full IntelliSense :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

La norme WAI-ARIA introduit également des attributs préfixés avec *de l’air,* qui vous permettent d’ajouter de la sémantique à un document HTML5. Visual Studio prend également en charge pleinement ces *aria-attributs* :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nouveaux extraits HTML5

Pour rendre plus rapide et plus facile d’écrire la balisage HTML5 couramment utilisé, Visual Studio comprend un certain nombre de extraits. Un exemple est l’extrait vidéo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Pour invoquer l’extrait, appuyez sur Tab deux fois lorsque l’élément est sélectionné dans IntelliSense :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Cela produit un extrait que vous pouvez personnaliser.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extrait au contrôle de l’utilisateur

Dans les grandes pages Web, il peut être une bonne idée de déplacer des pièces individuelles dans les contrôles des utilisateurs. Cette forme de refactorisation peut aider à augmenter la lisibilité de la page et peut simplifier la structure de la page.

Pour faciliter les choses, lorsque vous modifiez les pages Web Forms dans Source View, vous pouvez maintenant sélectionner du texte dans une page, cliquer à droite, puis choisir Extrait pour le contrôle utilisateur :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense pour pépites de code dans les attributs

Visual Studio a toujours fourni IntelliSense pour les pépites de code côté serveur dans n’importe quelle page ou contrôle. Maintenant Visual Studio inclut IntelliSense pour les pépites de code dans les attributs HTML ainsi.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Il est ainsi plus facile de créer des expressions liant des données :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Renommage automatique de l’étiquette assortie lorsque vous renommez une balise d’ouverture ou de fermeture

Si vous renommez un élément HTML (par exemple, vous modifiez une balise *div* pour être une étiquette *d’en-tête),* l’étiquette d’ouverture ou de fermeture correspondante change également en temps réel.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Cela permet d’éviter l’erreur où vous oubliez de changer une balise de clôture ou de changer la mauvaise.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Génération de gestionnaire d’événements

Visual Studio inclut maintenant des fonctionnalités dans Source view pour vous aider à écrire des gestionnaires d’événements et à les lier manuellement. Si vous modifiez un nom d’événement dans &lt;Source&gt;view, IntelliSense affiche Create New Event , qui créera un gestionnaire d’événements dans le code de la page qui a la bonne signature :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Par défaut, le gestionnaire d’événements utilisera l’ID du contrôle pour le nom de la méthode de manipulation de l’événement :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Le gestionnaire d’événements qui en résultera ressemblera à ceci (dans ce cas, dans C) :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Indent intelligent

Lorsque vous appuyez sur Enter à l’intérieur d’un élément HTML vide, l’éditeur mettra le point d’insertion au bon endroit :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Si vous appuyez sur Enter à cet endroit, l’étiquette de clôture est déplacé vers le bas et en retrait pour correspondre à l’étiquette d’ouverture. Le point d’insertion est également en retrait :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Achèvement de l’instruction auto-réduction

La liste IntelliSense dans Visual Studio filtre désormais en fonction de ce que vous tapez afin qu’il affiche uniquement les options pertinentes:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense filtre également en fonction du cas titre des mots individuels de la liste IntelliSense. Par exemple, si vous tapez "dl", dl et asp:DataList sont affichés:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Cette fonctionnalité permet d’obtenir plus rapidement l’achèvement de l’instruction pour les éléments connus.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>éditeur de code JavaScript

L’éditeur JavaScript dans Visual Studio 2012 Release Candidate est complètement nouveau et il améliore considérablement l’expérience de travailler avec JavaScript dans Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Plan du code

Les régions de base sont maintenant automatiquement créées pour toutes les fonctions, vous permettant d’effondrer des parties du fichier qui ne sont pas pertinentes à votre objectif actuel.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Correspondance d’accolade

Lorsque vous mettez le point d’insertion sur une accolade d’ouverture ou de fermeture, l’éditeur met en évidence l’appariement.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Atteindre la définition

La commande Go to Definition vous permet de sauter à la source pour une fonction ou une variable.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Soutien ECMAScript5

L’éditeur prend en charge la nouvelle syntaxe et API dans ECMAScript5, la dernière version de la norme qui décrit la langue JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

IntelliSense pour les API DOM a été améliorée, avec le soutien de nombreux nouveaux API HTML5, y compris *la requêteSelector*, DOM Storage, la messagerie multi-documents, et *la toile*. DOM IntelliSense est maintenant piloté par un seul fichier JavaScript simple, plutôt que par une définition de bibliothèque de type natif. Il est donc facile d’étendre ou de remplacer.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Surcharges de signature VSDOC

Les commentaires détaillés d’IntelliSense peuvent maintenant être déclarés pour * &lt;&gt; * des surcharges séparées des fonctions JavaScript en utilisant le nouvel élément de signature, comme indiqué dans cet exemple :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Références implicites

Vous pouvez maintenant ajouter des fichiers JavaScript à une liste centrale qui sera implicitement incluse dans la liste des fichiers que n’importe quel fichier JavaScript donné ou des références de bloc, ce qui signifie que vous obtiendrez IntelliSense pour son contenu. Par exemple, vous pouvez ajouter des fichiers jQuery à la liste centrale des fichiers, et vous obtiendrez IntelliSense pour les fonctions jQuery &lt;dans&gt;n’importe quel bloc de fichier JavaScript, que vous l’ayez référencé explicitement (en utilisant /// référence / ) ou non.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>éditeur CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Achèvement de l’instruction auto-réduction

La liste IntelliSense pour CSS filtre désormais en fonction des propriétés et des valeurs du CSS supportées par le schéma sélectionné.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense prend également en charge les recherches de cas de titre :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Mise en retrait hiérarchique

L’éditeur du CSS utilise l’indentation pour afficher des règles hiérarchiques, ce qui vous donne un aperçu de la façon dont les règles en cascade sont logiquement organisées. Dans l’exemple suivant, le #list’un sélecteur est un enfant en cascade de la liste et est donc en retrait.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

L’exemple suivant montre un héritage plus complexe :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

L’indentation d’une règle est déterminée par les règles de ses parents. L’indentation hiérarchique est activée par défaut, mais vous pouvez la désactiver la boîte de dialogue Options (Outils, Options de la barre de menu) :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS hacks support

L’analyse de centaines de fichiers CSS du monde réel montre que les hacks CSS sont très fréquents, et maintenant Visual Studio prend en charge les plus largement utilisés. Ce support inclut IntelliSense et\*la validation\_de l’étoile ( ) et de souligner ( ) hacks propriété:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Les hacks typiques de sélecteur sont également pris en charge de sorte que l’indentation hiérarchique soit maintenue même quand elles sont appliquées. Un hack sélecteur typique utilisé pour cibler Internet Explorer 7 est de prépendrer un sélecteur avec * \*:premier enfant - html*. L’utilisation de cette règle maintiendra l’indentation hiérarchique :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Schémas spécifiques au fournisseur (-moz-, -webkit)

CSS3 introduit de nombreuses propriétés qui ont été implémentées par différents navigateurs à des moments différents. Cela a déjà forcé les développeurs à coder pour des navigateurs spécifiques en utilisant la syntaxe spécifique au fournisseur. Ces propriétés spécifiques au navigateur sont maintenant incluses dans IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Commentaire et soutien inconvenant

Vous pouvez maintenant commenter et désengager les règles CSS en utilisant les mêmes raccourcis que vous utilisez dans l’éditeur de code (Ctrl-K, C pour commenter et Ctrl-K, vous à désengagement).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Color picker

Dans les versions précédentes de Visual Studio, IntelliSense pour les attributs liés aux couleurs se composait d’une liste de baisse des valeurs de couleur nommées. Cette liste a été remplacée par un cueilleur de couleurs en vedette.

Lorsque vous entrez une valeur de couleur, le cueilleur de couleurs est affiché automatiquement et présente une liste de couleurs précédemment utilisées suivie d’une palette de couleurs par défaut. Vous pouvez sélectionner une couleur à l’aide de la souris ou du clavier.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

La liste peut être élargie dans un cueilleur de couleurs complet. Le cueilleur vous permet de contrôler le canal alpha en convertissant automatiquement n’importe quelle couleur en RGBA lorsque vous déplacez le curseur d’opacité :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Extraits de code

Les extraits de l’éditeur CSS facilitent et accélèrent la création de styles transversaux. De nombreuses propriétés CSS3 qui nécessitent des paramètres spécifiques au navigateur ont maintenant été roulées en extraits.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Les extraits du CSS prennent en charge des scénarios avancés (comme les requêtes multimédias CSS3) en tapant le symbole ( ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' ' '

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Lorsque vous @media sélectionnez la valeur et appuyez sur Tab, l’éditeur CSS insère l’extrait suivant :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Comme avec des extraits de code, vous pouvez créer vos propres extraits CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Régions personnalisées

Les régions de code nommées, qui sont déjà disponibles dans l’éditeur de code, sont maintenant disponibles pour l’édition CSS. Cela vous permet de regrouper facilement les blocs de style connexes.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Lorsqu’une région est effondrée, elle affiche le nom de la région :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspecteur de page

Page Inspector est un outil qui rend une page Web (HTML, formulaires Web, ASP.NET MVC, ou Pages Web) dans l’IDE Visual Studio et vous permet d’examiner à la fois le code source et la sortie qui en résulte. Pour ASP.NET pages, Page Inspector vous permet de déterminer quel code côté serveur a produit la balisage HTML qui est rendu au navigateur.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Pour plus d’informations sur Page Inspector, veuillez consulter les tutoriels suivants :

- Utilisation de l’inspecteur de page dans [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Utilisation de l’inspecteur de page dans [ASP.NET formulaires Web](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publication

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Profils de publication

Dans Visual Studio 2010, la publication d’informations pour les projets d’applications Web n’est pas stockée dans le contrôle de la version et n’est pas conçue pour le partage avec d’autres. Dans Visual Studio 2012 Release Candidate, le format du profil de publication a été modifié. Il a été fait un artefact d’équipe, et il est maintenant facile de tirer parti des builds basés sur MSBuild. Les informations de configuration de construction sont dans la boîte de dialogue Publish afin que vous puissiez facilement changer de configuration de construction avant de la publier.

Les profils de publication sont stockés dans le dossier PublishProfiles. L’emplacement du dossier dépend du langage de programmation que vous utilisez :

- C : Propriétés-PublierProfiles
- Visual Basic: My Project-PublishProfiles

Chaque profil est un fichier MSBuild. Lors de la publication, ce fichier est importé dans le fichier MSBuild du projet. Dans Visual Studio 2010, si vous souhaitez apporter des modifications au processus de publication ou de package, vous devez mettre vos personnalisations dans un fichier nommé **ProjectName**.wpp.targets. Ceci est toujours pris en charge, mais vous pouvez maintenant mettre vos personnalisations dans le profil de publication lui-même. De cette façon, les personnalisations ne seront utilisées que pour ce profil.

Vous pouvez désormais également tirer parti des profils de publication de MSBuild. Pour ce faire, utilisez la commande suivante lorsque vous construisez le projet :

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

La valeur project.csproj est le chemin du projet, et ProfileName est le nom du profil à publier. Alternativement, au lieu de passer le nom de profil de la propriété *PublishProfile,* vous pouvez passer dans le chemin complet vers le profil de publication.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET précompatation et fusion

Pour les projets d’applications Web, Visual Studio 2012 Release Candidate ajoute une option sur la page propriétés Web De paquet/publication qui vous permet de pré-compiler et de fusionner le contenu de votre site lorsque vous publiez ou emballez le projet. Pour voir ces options, cliquez à droite sur le projet dans Solution Explorer, choisissez des propriétés, puis choisissez la page de propriété Web de paquet/publication. L’illustration suivante montre le Précompile cette application avant de publier l’option.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Lorsque cette option est sélectionnée, Visual Studio précalcite l’application chaque fois que vous publiez ou emballez l’application Web. Si vous souhaitez contrôler la façon dont le site est précompilé ou comment les assemblages sont fusionnés, cliquez sur le bouton Advanced pour configurer ces options.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Le serveur Web par défaut pour tester des projets web dans Visual Studio est maintenant IIS Express. Le serveur de développement visual studio est toujours une option pour le serveur web local pendant le développement, mais IIS Express est maintenant le serveur recommandé. L’expérience de l’utilisation d’IIS Express dans Visual Studio 11 Beta est très similaire à son utilisation dans Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Clause d'exclusion de responsabilité

Ce document est une version préliminaire et peut être modifié substantiellement avant le lancement de la mise en production commerciale finale du logiciel qu’il décrit.

Les informations contenues dans le présent document reflètent l'opinion de Microsoft Corporation sur les sujets abordés à la date de publication. Microsoft se doit de s'adapter aux conditions fluctuantes du marché, et cette opinion ne peut être considérée comme un engagement de sa part. Microsoft ne peut garantir la véracité de toute information présentée après la date de publication.

Ce livre blanc est fourni à titre d'information uniquement. MICROSOFT NE FOURNIT AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AUX INFORMATIONS CONTENUES DANS CE DOCUMENT.

L’utilisateur est tenu d’observer la réglementation relative aux droits d’auteur applicable dans son pays. Aucune partie de ce document ne peut être reproduite, stockée ou introduite dans un système de restitution, ou transmise à quelque fin ou par quelque moyen que ce soit (électronique, mécanique, photocopie, enregistrement ou autre) sans la permission expresse et écrite de Microsoft Corporation.

Les produits mentionnés dans ce document peuvent faire l'objet de brevets, de dépôts de brevets en cours, de marques, de droits d'auteur ou d'autres droits de propriété intellectuelle et industrielle de Microsoft. Sauf stipulation expresse contraire d'un contrat de licence écrit de Microsoft, la fourniture de ce document n'a pas pour effet de vous concéder une licence sur ces brevets, marques, droits d'auteur ou autres droits de propriété intellectuelle.

Sauf indication contraire, l’exemple des entreprises, des organisations, des produits, des noms de domaine, des adresses e-mail, des logos, des personnes, des lieux et des événements décrits ici sont fictifs, et aucune association avec une entreprise réelle, organisation, produit, nom de domaine, adresse e-mail, logo, personne, lieu ou événement est destiné ou devrait être déduit.

© 2012 Microsoft Corporation. Tous droits réservés.

Microsoft et Windows sont soit des marques déposées de Microsoft Corporation, soit des marques de Microsoft Corporation aux États-Unis d'Amérique et/ou dans d'autres pays.

Les noms des sociétés et des produits mentionnés dans le présent document peuvent être des marques de leurs propriétaires respectifs.
