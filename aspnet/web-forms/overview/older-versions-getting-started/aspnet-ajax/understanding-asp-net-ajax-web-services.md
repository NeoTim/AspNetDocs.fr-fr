---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Fonctionnement des services Web ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Les services Web sont une partie intégrante du .NET Framework qui fournit une solution multiplateforme pour l’échange de données entre des systèmes distribués. Bien que Web...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: eac3d53fd871d0cb5a2870488ce752c057cc5b1a
ms.sourcegitcommit: 45754124123403520b9fa2e706a4d1292494159b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/12/2020
ms.locfileid: "86162792"
---
# <a name="understanding-aspnet-ajax-web-services"></a>Présentation des services web ASP.NET AJAX

par [Scott Cate](https://github.com/scottcate)

[Télécharger le PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Les services Web sont une partie intégrante du .NET Framework qui fournit une solution multiplateforme pour l’échange de données entre des systèmes distribués. Bien que les services Web soient normalement utilisés pour permettre aux différents systèmes d’exploitation, modèles d’objet et langages de programmation d’envoyer et de recevoir des données, ils peuvent également être utilisés pour injecter dynamiquement des données dans une page AJAX ASP.NET ou envoyer des données d’une page à un système principal. Tout cela peut être fait sans avoir recours à des opérations de publication (postback).

## <a name="calling-web-services-with-aspnet-ajax"></a>Appel de services Web avec ASP.NET AJAX

Dan Wahlin

Les services Web sont une partie intégrante du .NET Framework qui fournit une solution multiplateforme pour l’échange de données entre des systèmes distribués. Bien que les services Web soient normalement utilisés pour permettre aux différents systèmes d’exploitation, modèles d’objet et langages de programmation d’envoyer et de recevoir des données, ils peuvent également être utilisés pour injecter dynamiquement des données dans une page AJAX ASP.NET ou envoyer des données d’une page à un système principal. Tout cela peut être fait sans avoir recours à des opérations de publication (postback).

Tandis que le contrôle UpdatePanel ASP.NET AJAX offre un moyen simple pour AJAX d’activer n’importe quelle page ASP.NET, il peut arriver que vous deviez accéder dynamiquement aux données sur le serveur sans utiliser d’UpdatePanel. Dans cet article, vous allez découvrir comment effectuer cette opération en créant et en consommant des services Web dans des pages AJAX ASP.NET.

Cet article se concentre sur les fonctionnalités disponibles dans les extensions ASP.NET AJAX principales, ainsi que sur le contrôle activé pour les services Web dans la boîte à outils ASP.NET AJAX appelée AutoCompleteExtender. Les sujets abordés incluent la définition de services Web compatibles AJAX, la création de proxys clients et l’appel de services Web avec JavaScript. Vous verrez également comment les appels de service Web peuvent être effectués directement sur les méthodes de page ASP.NET.

## <a name="web-services-configuration"></a>Configuration des services Web

Quand un nouveau projet de site Web est créé avec Visual Studio 2008, le fichier web.config présente un certain nombre de nouveaux ajouts qui peuvent ne pas être familiers pour les utilisateurs des versions antérieures de Visual Studio. Certaines de ces modifications mappent le préfixe « asp » aux contrôles ASP.NET AJAX pour qu’ils puissent être utilisés dans les pages, tandis que d’autres définissent les gestionnaires de script et les HttpModules requis. La liste 1 montre les modifications apportées à l' `<httpHandlers>` élément dans web.config qui affecte les appels de service Web. Le gestionnaire HttpHandler par défaut utilisé pour traiter les appels. asmx est supprimé et remplacé par une classe ScriptHandlerFactory située dans l’assembly System.Web.Extensions.dll. System.Web.Extensions.dll contient toutes les fonctionnalités de base utilisées par ASP.NET AJAX.

**Liste 1. Configuration du gestionnaire de service Web ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Ce remplacement de HttpHandler est effectué afin d’autoriser les appels JavaScript Object Notation (JSON) à être effectués à partir de pages AJAX ASP.NET vers les services Web .NET à l’aide d’un proxy de service Web JavaScript. ASP.NET AJAX envoie des messages JSON aux services Web, par opposition aux appels SOAP (Simple Object Access Protocol) standard généralement associés aux services Web. Cela génère des messages de demande et de réponse plus petits. Il permet également un traitement des données plus efficace côté client, car la bibliothèque JavaScript ASP.NET AJAX est optimisée pour fonctionner avec des objets JSON. La liste 2 et la liste 3 montrent des exemples de messages de demande et de réponse de service Web sérialisés au format JSON. Le message de demande affiché dans la liste 2 transmet un paramètre Country avec la valeur « Belgium », tandis que le message de réponse de la liste 3 passe un tableau d’objets Customer et leurs propriétés associées.

**Liste 2. Message de requête de service Web sérialisé en JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *>[!NOTE]le nom de l’opération est défini dans le cadre de l’URL du service Web ; en outre, les messages de demande ne sont pas toujours envoyés via JSON. Les services Web peuvent utiliser l’attribut ScriptMethod avec le paramètre UseHttpGet défini sur true, ce qui entraîne le passage des paramètres via les paramètres de chaîne de requête.*

**Liste 3. Message de réponse du service Web sérialisé en JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Dans la section suivante, vous allez apprendre à créer des services Web aptes à gérer les messages de requête JSON et à répondre avec des types simples et complexes.

## <a name="creating-ajax-enabled-web-services"></a>Création de services Web compatibles AJAX

L’infrastructure ASP.NET AJAX offre différentes façons d’appeler des services Web. Vous pouvez utiliser le contrôle AutoCompleteExtender (disponible dans la boîte à outils ASP.NET AJAX) ou JavaScript. Toutefois, avant d’appeler un service, vous devez l’activer pour qu’il puisse être appelé par du code de script client.

Que vous soyez ou non un nouvel ASP.NET des services Web, vous pouvez facilement créer et activer des services AJAX. Le .NET Framework a pris en charge la création de services Web ASP.NET depuis sa version initiale dans 2002 et les extensions AJAX ASP.NET fournissent des fonctionnalités AJAX supplémentaires qui s’appuient sur l’ensemble de fonctionnalités par défaut du .NET Framework. Visual Studio .NET 2008 bêta 2 offre une prise en charge intégrée de la création de fichiers de service Web. asmx et dérive automatiquement le code associé à côté des classes de la classe System. Web. services. WebService. À mesure que vous ajoutez des méthodes à la classe, vous devez appliquer l’attribut WebMethod pour qu’elles soient appelées par les consommateurs du service Web.

La liste 4 montre un exemple d’application de l’attribut WebMethod à une méthode nommée GetCustomersByCountry ().

**Liste 4. Utilisation de l’attribut WebMethod dans un service Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

La méthode GetCustomersByCountry () accepte un paramètre Country et retourne un tableau d’objets Customer. La valeur de pays transmise dans la méthode est transférée à une classe de couche métier qui, à son tour, appelle une classe de couche de données pour récupérer les données de la base de données, remplir les propriétés de l’objet Customer avec des données et retourner le tableau.

## <a name="using-the-scriptservice-attribute"></a>Utilisation de l’attribut ScriptService

Tandis que l’ajout de l’attribut WebMethod permet à la méthode GetCustomersByCountry () d’être appelée par les clients qui envoient des messages SOAP standard au service Web, elle n’autorise pas les appels JSON à partir des applications ASP.NET AJAX prêtes à l’emploi. Pour permettre l’établissement d’appels JSON, vous devez appliquer l’attribut de l’extension ASP.NET AJAX `ScriptService` à la classe de service Web. Cela permet à un service Web d’envoyer des messages de réponse au format JSON et permet au script côté client d’appeler un service en envoyant des messages JSON.

La liste 5 montre un exemple d’application de l’attribut ScriptService à une classe de service Web nommée CustomersService.

**Liste 5. Utilisation de l’attribut ScriptService pour AJAX-activer un service Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

L’attribut ScriptService agit comme un marqueur qui indique qu’il peut être appelé à partir du code de script AJAX. Elle ne gère en fait aucune des tâches de sérialisation ou de désérialisation JSON qui se produisent en arrière-plan. Le ScriptHandlerFactory (configuré dans web.config) et d’autres classes associées effectuent la majeure partie du traitement JSON.

## <a name="using-the-scriptmethod-attribute"></a>Utilisation de l’attribut ScriptMethod

L’attribut ScriptService est le seul attribut AJAX ASP.NET qui doit être défini dans un service Web .NET afin qu’il soit utilisé par les pages AJAX ASP.NET. Toutefois, un autre attribut nommé ScriptMethod peut également être appliqué directement aux méthodes Web dans un service. ScriptMethod définit trois propriétés `UseHttpGet` , notamment, `ResponseFormat` et `XmlSerializeString` . La modification des valeurs de ces propriétés peut être utile dans les cas où le type de requête accepté par une méthode Web doit être remplacé par obtenir, lorsqu’une méthode Web doit retourner des données XML brutes sous la forme d’un `XmlDocument` `XmlElement` objet ou ou lorsque les données retournées par un service doivent toujours être sérialisées au format XML au lieu de JSON.

La propriété UseHttpGet peut être utilisée lorsqu’une méthode Web doit accepter les demandes d’extraction, par opposition aux demandes de publication. Les demandes sont envoyées à l’aide d’une URL avec des paramètres d’entrée de méthode Web convertis en Paramètres QueryString. La valeur par défaut de la propriété UseHttpGet est false et ne doit être définie que `true` lorsque les opérations sont connues comme sécurisées et lorsque les données sensibles ne sont pas passées à un service Web. La liste 6 illustre un exemple d’utilisation de l’attribut ScriptMethod avec la propriété UseHttpGet.

**Liste 6. Utilisation de l’attribut ScriptMethod avec la propriété UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Voici un exemple d’en-têtes envoyés lorsque la méthode Web HttpGetEcho présentée dans la liste 6 est appelée suivante :

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

En plus de permettre aux méthodes Web d’accepter les requêtes HTTP, l’attribut ScriptMethod peut également être utilisé lorsque les réponses XML doivent être retournées à partir d’un service plutôt que JSON. Par exemple, un service Web peut récupérer un flux RSS à partir d’un site distant et le renvoyer sous la forme d’un objet XmlDocument ou XmlElement. Le traitement des données XML peut alors se produire sur le client.

La liste 7 montre un exemple d’utilisation de la propriété ResponseFormat pour spécifier que les données XML doivent être retournées à partir d’une méthode Web.

**Liste 7. Utilisation de l’attribut ScriptMethod avec la propriété ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

La propriété ResponseFormat peut également être utilisée avec la propriété XmlSerializeString. La valeur par défaut de la propriété XmlSerializeString est false, ce qui signifie que tous les types de retour, à l’exception des chaînes retournées à partir d’une méthode Web, sont sérialisés au format XML lorsque la `ResponseFormat` propriété a la valeur `ResponseFormat.Xml` . Lorsque `XmlSerializeString` a la valeur `true` , tous les types retournés à partir d’une méthode Web sont sérialisés au format XML, y compris les types chaîne. Si la propriété ResponseFormat a une valeur de `ResponseFormat.Json` la propriété XmlSerializeString est ignorée.

La liste 8 illustre un exemple d’utilisation de la propriété XmlSerializeString pour forcer la sérialisation des chaînes au format XML.

**Liste 8. Utilisation de l’attribut ScriptMethod avec la propriété XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

La valeur retournée par l’appel de la méthode Web GetXmlString présentée dans la liste 8 est indiquée ci-dessous :

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Bien que le format JSON par défaut minimise la taille globale des messages de demande et de réponse et qu’il soit plus facilement utilisé par les clients ASP.NET AJAX de manière internavigateur, les propriétés ResponseFormat et XmlSerializeString peuvent être utilisées lorsque des applications clientes telles qu’Internet Explorer 5 ou version ultérieure attendent que les données XML soient retournées à partir d’une méthode Web.

## <a name="working-with-complex-types"></a>Utilisation des types complexes

La liste 5 a montré un exemple de retour d’un type complexe nommé Customer à partir d’un service Web. La classe Customer définit plusieurs types simples différents en interne en tant que propriétés telles que FirstName et LastName. Les types complexes utilisés comme un paramètre d’entrée ou un type de retour sur une méthode Web compatible AJAX sont automatiquement sérialisés en JSON avant d’être envoyés côté client. Toutefois, les types complexes imbriqués (ceux définis en interne dans un autre type) ne sont pas mis à la disposition du client en tant qu’objets autonomes par défaut.

Dans les cas où un type complexe imbriqué utilisé par un service Web doit également être utilisé dans une page cliente, l’attribut ASP.NET AJAX GenerateScriptType peut être ajouté au service Web. Par exemple, la classe CustomerDetails indiquée dans la liste 9 contient des propriétés adresse et sexe qui ***représentent des types complexes imbriqués.***

**Liste 9. La classe CustomerDetails illustrée ici contient deux types complexes imbriqués.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Les objets adresse et sexe définis dans la classe CustomerDetails indiquée dans la liste 9 ne seront pas automatiquement disponibles pour être utilisés côté client via JavaScript, car il s’agit de types imbriqués (l’adresse est une classe et le sexe est une énumération). Dans les situations où un type imbriqué utilisé dans un service Web doit être disponible côté client, l’attribut GenerateScriptType mentionné précédemment peut être utilisé (voir le Listing 10). Cet attribut peut être ajouté plusieurs fois dans les cas où différents types complexes imbriqués sont retournés à partir d’un service. Il peut être appliqué directement à la classe de service Web ou à des méthodes Web spécifiques.

**Liste 10. Utilisation de l’attribut GenerateScriptService pour définir les types imbriqués qui doivent être disponibles pour le client.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

En appliquant l' `GenerateScriptType` attribut au service Web, les types d’adresse et de sexe deviennent automatiquement disponibles pour une utilisation par le code JavaScript ASP.NET AJAX côté client. Un exemple du code JavaScript qui est généré et envoyé automatiquement au client en ajoutant l’attribut GenerateScriptType sur un service Web est présenté dans la liste 11. Vous verrez comment utiliser les types complexes imbriqués plus loin dans cet article.

**Liste 11. Types complexes imbriqués mis à disposition d’une page AJAX ASP.NET.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Maintenant que vous avez vu comment créer des services Web et les rendre accessibles aux pages AJAX ASP.NET, voyons comment créer et utiliser des proxies JavaScript afin que les données puissent être extraites ou envoyées aux services Web.

## <a name="creating-javascript-proxies"></a>Création de proxies JavaScript

L’appel d’un service Web standard (.NET ou une autre plateforme) implique généralement la création d’un objet proxy qui vous protège des complexités liées à l’envoi de messages de demande et de réponse SOAP. Avec les appels de service Web ASP.NET AJAX, les proxies JavaScript peuvent être créés et utilisés pour appeler facilement des services sans se soucier de la sérialisation et de la désérialisation des messages JSON. Les proxies JavaScript peuvent être générés automatiquement à l’aide du contrôle ASP.NET AJAX ScriptManager.

La création d’un proxy JavaScript qui peut appeler des services Web s’effectue à l’aide de la propriété services de ScriptManager. Cette propriété vous permet de définir un ou plusieurs services qu’une page AJAX ASP.NET peut appeler de façon asynchrone pour envoyer ou recevoir des données sans avoir besoin d’opérations de publication (postback). Vous définissez un service à l’aide du contrôle ASP.NET AJAX `ServiceReference` et en assignant l’URL du service Web à la propriété du contrôle `Path` . La liste 12 montre un exemple de référencement d’un service nommé CustomersService. asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Liste 12. Définition d’un service Web utilisé dans une page AJAX ASP.NET.**

L’ajout d’une référence à CustomersService. asmx par le biais du contrôle ScriptManager entraîne la génération dynamique d’un proxy JavaScript et son référencement par la page. Le proxy est intégré à l’aide de la &lt; &gt; balise de script et est chargé dynamiquement en appelant le fichier CustomersService. asmx et en ajoutant/js à la fin de celui-ci. L’exemple suivant montre comment le proxy JavaScript est incorporé dans la page lorsque le débogage est désactivé dans web.config :

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *>[!NOTE]Si vous souhaitez voir le code proxy JavaScript réel qui est généré, vous pouvez taper l’URL du service Web .net souhaité dans la zone d’adresse d’Internet Explorer et ajouter/js à la fin de celui-ci.*

Si le débogage est activé dans web.config une version de débogage du proxy JavaScript sera incorporée dans la page comme indiqué ci-dessous :

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Le proxy JavaScript créé par ScriptManager peut également être incorporé directement dans la page plutôt que référencé à l’aide de l' &lt; &gt; attribut SRC de la balise de script. Pour ce faire, affectez à la propriété InlineScript du contrôle ServiceReference la valeur true (la valeur par défaut est false). Cela peut être utile lorsqu’un proxy n’est pas partagé entre plusieurs pages et que vous souhaitez réduire le nombre d’appels réseau effectués sur le serveur. Lorsque InlineScript a la valeur true, le script de proxy n’est pas mis en cache par le navigateur. par conséquent, la valeur par défaut false est recommandée dans les cas où le proxy est utilisé par plusieurs pages dans une application ASP.NET AJAX. Un exemple d’utilisation de la propriété InlineScript est illustré ci-dessous :

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Utilisation de proxys JavaScript

Une fois qu’un service Web est référencé par une page AJAX ASP.NET à l’aide du contrôle ScriptManager, un appel peut être fait au service Web et les données retournées peuvent être gérées à l’aide de fonctions de rappel. Un service Web est appelé en référençant son espace de noms (le cas échéant), le nom de la classe et le nom de la méthode Web. Tous les paramètres passés au service Web peuvent être définis avec une fonction de rappel qui gère les données retournées.

Un exemple d’utilisation d’un proxy JavaScript pour appeler une méthode Web nommée GetCustomersByCountry () est présenté dans la liste 13. La fonction GetCustomersByCountry () est appelée lorsqu’un utilisateur final clique sur un bouton de la page.

**Liste 13. Appel d’un service Web avec un proxy JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Cet appel fait référence à l’espace de noms InterfaceTraining, à la classe CustomersService et à la méthode Web GetCustomersByCountry définie dans le service. Elle passe une valeur de pays obtenue à partir d’une zone de texte, ainsi qu’une fonction de rappel nommée OnWSRequestComplete qui doit être appelée lorsque l’appel de service Web asynchrone retourne. OnWSRequestComplete gère le tableau d’objets Customer retournés par le service et les convertit en une table qui est affichée dans la page. La sortie générée à partir de l’appel est illustrée à la figure 1.

[![Liaison de données obtenues en effectuant un appel AJAX asynchrone à un service Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figure 1**: liaison de données obtenues en effectuant un appel Ajax asynchrone à un service Web.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-web-services/_static/image3.png))

Les proxies JavaScript peuvent également effectuer des appels unidirectionnels aux services Web dans les cas où une méthode Web doit être appelée, mais le proxy ne doit pas attendre une réponse. Par exemple, vous souhaiterez peut-être appeler un service Web pour démarrer un processus tel qu’un workflow, sans attendre une valeur de retour du service. Dans les cas où un appel unidirectionnel doit être effectué à un service, la fonction de rappel présentée dans la liste 13 peut tout simplement être omise. Étant donné qu’aucune fonction de rappel n’est définie, l’objet proxy n’attend pas que le service Web retourne des données.

## <a name="handling-errors"></a>Gestion des erreurs

Les rappels asynchrones aux services Web peuvent rencontrer différents types d’erreurs, tels que le réseau en cours d’indisponibilité, le service Web non disponible ou une exception retournée. Heureusement, les objets proxy JavaScript générés par ScriptManager autorisent la définition de plusieurs rappels pour gérer les erreurs et les échecs en plus du rappel de réussite présenté précédemment. Une fonction de rappel d’erreur peut être définie immédiatement après la fonction de rappel standard dans l’appel à la méthode Web, comme indiqué dans la liste 14.

**Liste 14. Définition d’une fonction de rappel d’erreur et affichage des erreurs.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Toute erreur qui se produit lorsque le service Web est appelé déclenchera la fonction de rappel OnWSRequestFailed () à appeler, qui accepte un objet représentant l’erreur en tant que paramètre. L’objet Error expose plusieurs fonctions différentes pour déterminer la cause de l’erreur, et si l’appel a expiré ou non. La liste 14 illustre un exemple d’utilisation des différentes fonctions d’erreur et la figure 2 illustre un exemple de la sortie générée par les fonctions.

[![Sortie générée en appelant les fonctions d’erreur ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figure 2**: sortie générée par l’appel de fonctions d’erreur ASP.NET AJAX.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-web-services/_static/image6.png))

## <a name="handling-xml-data-returned-from-a-web-service"></a>Gestion des données XML retournées par un service Web

Précédemment, vous avez vu comment une méthode Web pourrait retourner des données XML brutes à l’aide de l’attribut ScriptMethod avec sa propriété ResponseFormat. Quand ResponseFormat a la valeur ResponseFormat.Xml, les données retournées à partir du service Web sont sérialisées au format XML plutôt que JSON. Cela peut être utile lorsque les données XML doivent être transmises directement au client pour traitement à l’aide de JavaScript ou de XSLT. À l’heure actuelle, Internet Explorer 5 ou version ultérieure fournit le meilleur modèle d’objet côté client pour l’analyse et le filtrage des données XML en raison de la prise en charge intégrée de MSXML.

La récupération de données XML à partir d’un service Web n’est pas différente de la récupération d’autres types de données. Commencez par appeler le proxy JavaScript pour appeler la fonction appropriée et définir une fonction de rappel. Une fois l’appel retourné, vous pouvez traiter les données dans la fonction de rappel.

La liste 15 montre un exemple d’appel d’une méthode Web nommée GetRssFeed () qui retourne un objet XmlElement. GetRssFeed () accepte un seul paramètre représentant l’URL du flux RSS à récupérer.

**Liste 15. Utilisation des données XML retournées par un service Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Cet exemple passe une URL à un flux RSS et traite les données XML renvoyées dans la fonction OnWSRequestComplete (). OnWSRequestComplete () vérifie d’abord si le navigateur est Internet Explorer pour savoir si l’analyseur MSXML est disponible. Si c’est le cas, une instruction XPath est utilisée pour localiser toutes les &lt; &gt; balises d’élément dans le flux RSS. Chaque élément est ensuite parcouru et le &lt; titre &gt; et les &lt; balises de lien associés &gt; sont localisés et traités pour afficher les données de chaque élément. La figure 3 montre un exemple de la sortie générée par l’exécution d’un appel AJAX ASP.NET via un proxy JavaScript vers la méthode Web GetRssFeed ().

## <a name="handling-complex-types"></a>Gestion des types complexes

Les types complexes acceptés ou retournés par un service Web sont automatiquement exposés via un proxy JavaScript. Toutefois, les types complexes imbriqués ne sont pas directement accessibles côté client, sauf si l’attribut GenerateScriptType est appliqué au service, comme indiqué précédemment. Pourquoi souhaiteriez-vous utiliser un type complexe imbriqué côté client ?

Pour répondre à cette question, supposez qu’une page AJAX ASP.NET affiche les données client et permet aux utilisateurs finaux de mettre à jour l’adresse d’un client. Si le service Web spécifie que le type d’adresse (un type complexe défini dans une classe CustomerDetails) peut être envoyé au client, le processus de mise à jour peut être divisé en fonctions distinctes pour une meilleure réutilisation du code.

[![Résultat de la création à partir de l’appel d’un service Web qui retourne des données RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figure 3**: création d’une sortie à partir de l’appel d’un service Web qui retourne des données RSS.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-web-services/_static/image9.png))

La liste 16 montre un exemple de code côté client qui appelle un objet d’adresse défini dans un espace de noms de modèle, le remplit avec des données mises à jour et l’attribue à la propriété d’adresse d’un objet CustomerDetails. L’objet CustomerDetails est ensuite transmis au service Web pour traitement.

**Liste 16. Utilisation de types complexes imbriqués**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Création et utilisation de méthodes de page

Les services Web fournissent un excellent moyen d’exposer les services réutilisables à un large éventail de clients, notamment les pages AJAX ASP.NET. Toutefois, il peut arriver qu’une page doive récupérer des données qui ne seront jamais utilisées ou partagées par d’autres pages. Dans ce cas, le fait de créer un fichier. asmx pour permettre à la page d’accéder aux données peut paraître excessif puisque le service n’est utilisé que par une seule page.

ASP.NET AJAX fournit un autre mécanisme pour effectuer des appels de type service Web sans créer de fichiers. asmx autonomes. Cette opération s’effectue à l’aide d’une technique appelée « méthodes de page ». Les méthodes de page sont des méthodes statiques (partagées dans VB.NET) incorporées directement dans une page ou un fichier code-beside auquel l’attribut WebMethod est appliqué. En appliquant l’attribut WebMethod, ils peuvent être appelés à l’aide d’un objet JavaScript spécial nommé PageMethods qui est créé dynamiquement au moment de l’exécution. L’objet PageMethods agit comme un proxy qui vous protège du processus de sérialisation/désérialisation JSON. Notez que pour pouvoir utiliser l’objet PageMethods, vous devez affecter la valeur true à la propriété EnablePageMethods de ScriptManager.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

La liste 17 montre un exemple de définition de deux méthodes de page dans une classe ASP.NET code-beside. Ces méthodes récupèrent des données à partir d’une classe de couche métier située dans le \_ dossier du code de l’application du site Web.

**Liste 17. Définition des méthodes de page.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Lorsque ScriptManager détecte la présence de méthodes Web dans la page, il génère une référence dynamique à l’objet PageMethods mentionné précédemment. L’appel d’une méthode Web s’effectue en référençant la classe PageMethods suivie du nom de la méthode et des données de paramètre nécessaires qui doivent être passées. La liste 18 montre des exemples d’appel des deux méthodes de page indiquées précédemment.

**Liste 18. Appel des méthodes de page avec l’objet JavaScript PageMethods.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

L’utilisation de l’objet PageMethods est très similaire à l’utilisation d’un objet proxy JavaScript. Vous devez d’abord spécifier toutes les données de paramètres qui doivent être passées à la méthode de page, puis définir la fonction de rappel qui doit être appelée lorsque l’appel asynchrone est retourné. Un rappel d’échec peut également être spécifié (reportez-vous à la liste 14 pour obtenir un exemple de gestion des échecs).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender et ASP.NET AJAX Toolkit

ASP.NET AJAX Toolkit (disponible à partir de [http://ajax.asp.net](http://ajax.asp.net) ) offre plusieurs contrôles qui peuvent être utilisés pour accéder aux services Web. Plus précisément, la boîte à outils contient un contrôle utile nommé `AutoCompleteExtender` qui peut être utilisé pour appeler des services Web et afficher des données dans des pages sans écrire de code JavaScript.

Le contrôle AutoCompleteExtender peut être utilisé pour étendre les fonctionnalités existantes d’une zone de texte et aider les utilisateurs à localiser plus facilement les données qu’ils recherchent. Lorsqu’ils tapent dans une zone de texte, le contrôle peut être utilisé pour interroger un service Web et afficher dynamiquement les résultats sous la zone de texte. La figure 4 illustre un exemple d’utilisation du contrôle AutoCompleteExtender pour afficher les ID client d’une application de support. À mesure que l’utilisateur tape des caractères différents dans la zone de texte, des éléments différents s’affichent en dessous de l’entrée. Les utilisateurs peuvent ensuite sélectionner l’ID de client de votre choix.

L’utilisation de AutoCompleteExtender dans une page ASP.NET AJAX requiert l’ajout de l’assembly AjaxControlToolkit.dll au dossier bin du site Web. Une fois l’assembly du Toolkit ajouté, vous pouvez le référencer dans web.config afin que les contrôles qu’il contient soient disponibles pour toutes les pages d’une application. Pour ce faire, vous pouvez ajouter la balise suivante dans la &lt; balise des contrôles de web.config &gt; :

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Dans les cas où vous devez uniquement utiliser le contrôle dans une page spécifique, vous pouvez le référencer en ajoutant la directive de référence en haut d’une page, comme indiqué ci-dessous, plutôt que de mettre à jour web.config :

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]

[![Utilisation du contrôle AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figure 4**: utilisation du contrôle AutoCompleteExtender.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-web-services/_static/image12.png))

Une fois que le site Web a été configuré pour utiliser ASP.NET AJAX Toolkit, un contrôle AutoCompleteExtender peut être ajouté à la page de la même façon que vous ajoutez un contrôle serveur ASP.NET normal. La liste 19 montre un exemple d’utilisation du contrôle pour appeler un service Web.

**Liste 19. Utilisation du contrôle AutoCompleteExtender de la boîte à outils ASP.NET AJAX.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender a plusieurs propriétés, y compris l’ID standard et les propriétés runat qui se trouvent sur les contrôles serveur. En outre, il vous permet de définir le nombre de caractères qu’un utilisateur final tape avant que le service Web soit interrogé pour obtenir des données. La propriété MinimumPrefixLength indiquée dans la liste 19 provoque l’appel du service chaque fois qu’un caractère est tapé dans la zone de texte. Vous devez définir cette valeur avec prudence, car chaque fois que l’utilisateur tape un caractère, le service Web est appelé pour rechercher les valeurs qui correspondent aux caractères de la zone de texte. Le service Web à appeler ainsi que la méthode Web cible sont définis à l’aide des propriétés ServicePath et ServiceMethod, respectivement. Enfin, la propriété TargetControlID identifie la zone de texte vers laquelle raccorder le contrôle AutoCompleteExtender.

L’attribut ScriptService doit être appliqué au service Web appelé, comme indiqué précédemment, et la méthode Web cible doit accepter deux paramètres nommés prefixText et Count. Le paramètre prefixText représente les caractères tapés par l’utilisateur final et le paramètre count représente le nombre d’éléments à retourner (la valeur par défaut est 10). La liste 20 montre un exemple de la méthode Web GetCustomerIDs appelée par le contrôle AutoCompleteExtender indiqué plus haut dans la liste 19. La méthode Web appelle une méthode de couche métier qui à son tour appelle une méthode de couche de données qui gère le filtrage des données et le retour des résultats de correspondance. Le code de la méthode de la couche de données est présenté dans la liste 21.

**Liste 20. Filtrage des données envoyées à partir du contrôle AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Liste 21. Filtrage des résultats en fonction de l’entrée de l’utilisateur final.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusion

ASP.NET AJAX offre une excellente prise en charge de l’appel de services Web sans écrire un grand nombre de code JavaScript personnalisé pour gérer les messages de demande et de réponse. Dans cet article, vous avez vu comment AJAX permet aux services Web .NET de les autoriser à traiter des messages JSON et à définir des proxies JavaScript à l’aide du contrôle ScriptManager. Vous avez également vu comment les proxies JavaScript peuvent être utilisés pour appeler des services Web, gérer des types simples et complexes et gérer les défaillances. Enfin, vous avez vu comment les méthodes de page peuvent être utilisées pour simplifier le processus de création et d’appel de service Web, ainsi que la façon dont le contrôle AutoCompleteExtender peut fournir de l’aide aux utilisateurs finaux au fur et à mesure de leur saisie. Bien que l’UpdatePanel disponible dans ASP.NET AJAX soit certainement le contrôle de choix pour de nombreux programmeurs AJAX en raison de sa simplicité, savoir comment appeler des services Web par le biais de proxys JavaScript peut être utile dans de nombreuses applications.

## <a name="bio"></a>Bio

Dan Wahlin (professionnel Microsoft le plus précieux pour ASP.NET et les services Web XML) est un formateur de développement .NET et un consultant en architecture à la formation technique d’interface ( [http://www.interfacett.com](http://www.interfacett.com) ). Dan a créé le site Web XML for ASP.NET Developers ([www.XMLforASP.net](http://www.XMLforASP.NET)), se trouve sur le Bureau du conférencier INETA et participe à plusieurs conférences. Dan co-auteur professionnel Windows DNA (Wrox), ASP.NET : conseils, didacticiels et code (Sams), ASP.NET 1,1 Insider solutions, professionnel ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 hackers MVP et auteur XML for ASP.NET Developers (Sams). Lorsqu’il n’écrit pas de code, d’articles ou de livres, Dan aime écrire et enregistrer de la musique et écouter du golf et du basket avec sa femme et ses enfants.

Scott Cate travaille avec les technologies Web de Microsoft depuis 1997 et est le Président de myKB.com ([www.myKB.com](http://www.myKB.com)), où il se spécialise dans l’écriture d’applications ASP.net axées sur les solutions logicielles de la base de connaissances. Scott peut être contacté par e-mail à l’adresse [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou sur son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-asp-net-ajax-localization.md) 
>  [Suivant](understanding-asp-net-ajax-debugging-capabilities.md)
