---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Prise en charge des Actions OData dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'Dans OData, les actions sont un moyen pour ajouter des comportements côté serveur qui ne sont pas facilement définies en tant qu’opérations CRUD sur les entités. Certaines utilisations pour les actions sont les suivantes : Implémentez...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: 71c0f91f709aca0deb5548bdbcad60d79a2702f6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57060176"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Prise en charge des Actions OData dans ASP.NET Web API 2
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Dans OData, *actions* sont un moyen pour ajouter des comportements côté serveur qui ne sont pas facilement définies en tant qu’opérations CRUD sur les entités. Certaines utilisations pour les actions sont les suivantes :
> 
> - Implémentation des transactions complexes.
> - Manipulation de plusieurs entités à la fois.
> - Ce qui permet des mises à jour uniquement certaines propriétés d’une entité.
> - Envoi d’informations sur le serveur qui n’est pas défini dans une entité.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - Web API 2
> - OData Version 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Exemple : Évaluation d’un produit

Dans cet exemple, nous voulons permettre aux utilisateurs de classer les produits, puis l’exposer l’évaluation moyenne pour chaque produit. Sur la base de données, nous allons stocker une liste de contrôle d’accès, la clé de produits.

Voici le modèle que nous pouvons utiliser pour représenter les évaluations dans Entity Framework :

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Mais nous ne voulons pas les clients pour valider un `ProductRating` objet à une collection de « Notes ». Intuitivement, l’évaluation est associée à la collection de produits, et le client ne doit valider la valeur d’évaluation.

Par conséquent, au lieu d’utiliser les opérations CRUD normales, nous définissons une action qu’un client peut appeler sur un produit. Dans la terminologie d’OData, l’action est *lié* aux entités du produit.

>Actions ont des effets sur le serveur. Pour cette raison, ils sont appelés à l’aide de requêtes HTTP POST. Actions peuvent avoir des paramètres et types de retour, qui sont décrites dans les métadonnées du service. Le client envoie les paramètres dans le corps de la demande, et le serveur envoie la valeur de retour dans le corps de réponse. Pour appeler l’action « Taux produit », le client envoie un POST à un URI comme suit :

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Les données dans la requête POST sont simplement l’évaluation de produit :

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Déclarez l’Action dans l’Entity Data Model

Dans la configuration de votre API Web, ajoutez l’action à l’entity data model (EDM) :

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Ce code définit « RateProduct » en tant qu’action qui peut être effectuée sur les entités Product. Elle déclare également que l’action prend un **int** paramètre nommé « Rating » et retourne un **int** valeur.

## <a name="add-the-action-to-the-controller"></a>Ajoutez l’Action au contrôleur

L’action « RateProduct » est liée aux entités de produit. Pour implémenter l’action, ajoutez une méthode nommée `RateProduct` au contrôleur de produits :

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Notez que le nom de méthode correspond au nom de l’action dans le modèle EDM. La méthode a deux paramètres :

- *Clé*: La clé du produit d’un taux.
- *Paramètres*: Dictionnaire de valeurs de paramètre d’action.

Si vous utilisez les conventions d’itinéraire par défaut, le paramètre de clé doit être nommé « clé ». Il est également important d’inclure le **[FromOdataUri]** d’attribut, comme indiqué. Cet attribut indique à l’API Web à utiliser les règles de syntaxe OData lorsqu’il analyse la clé à partir de l’URI de demande.

Utilisez le *paramètres* dictionnaire pour obtenir les paramètres d’action :

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Si le client envoie les paramètres d’action dans le bon format, la valeur de **ModelState.IsValid** a la valeur true. Dans ce cas, vous pouvez utiliser la **ODataActionParameters** dictionnaire pour obtenir les valeurs de paramètre. Dans cet exemple, le `RateProduct` action prend un paramètre unique nommé « Rating ».

## <a name="action-metadata"></a>Métadonnées d’action

Pour afficher les métadonnées du service, envoyez une requête GET à /odata/$ metadata. Voici la partie des métadonnées qui déclare le `RateProduct` action :

[!code-xml[Main](odata-actions/samples/sample7.xml)]

Le **FunctionImport** élément déclare l’action. La plupart des champs est explicite, mais les deux sont à noter :

- **IsBindable** signifie que l’action peut être appelée sur l’entité cible, au moins du temps.
- **IsAlwaysBindable** signifie que l’action peut toujours être appelée sur l’entité cible.

La différence est que certaines actions sont toujours disponibles pour les clients, mais les autres actions peuvent dépendre de l’état de l’entité. Par exemple, supposons que vous définissez une action « Acheter ». Vous pouvez acheter uniquement un élément qui est en stock. Si l’élément est en rupture de stock, un client ne peut pas appeler cette action.

Lorsque vous définissez le modèle EDM, le **Action** méthode crée une action toujours pouvant être lié à :

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Je vous parlerai pas toujours liable actions (également appelé *temporaires* actions) plus loin dans cette rubrique.

## <a name="invoking-the-action"></a>Appel d’Action

Maintenant nous allons voir comment un client appelle cette action. Supposons que le client veut donner une évaluation égale à 2 pour le produit avec l’ID = 4. Voici un exemple de message de demande, à l’aide du format JSON pour le corps de la demande :

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Voici le message de réponse :

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Liaison d’une Action à un jeu d’entités

Dans l’exemple précédent, l’action est liée à une seule entité : Le client évalue un produit unique. Vous pouvez également lier une action à une collection d’entités. Apportez simplement les modifications suivantes :

Dans le modèle EDM, ajoutez l’action à l’entité **Collection** propriété.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Dans la méthode de contrôleur, omettez la *clé* paramètre.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Désormais, le client appelle l’action sur le jeu d’entités de produits :

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Actions avec des paramètres de collecte

Actions peuvent avoir des paramètres qui acceptent une collection de valeurs. Dans le modèle EDM, utilisez **CollectionParameter&lt;T&gt;**  pour déclarer le paramètre.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Cela déclare un paramètre nommé « Évaluation » qui accepte une collection de **int** valeurs. Dans la méthode de contrôleur, vous obtenez toujours la valeur du paramètre à partir de la **ODataActionParameters** objet, mais désormais la valeur est un **ICollection&lt;int&gt;**  valeur :

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Actions temporaires

Dans l’exemple « RateProduct », les utilisateurs peuvent évaluer toujours un produit, par conséquent, l’action est toujours disponible. Mais certaines actions dépendent de l’état de l’entité. Par exemple, dans un service de location de vidéo, l’action « Récupération » n’est pas toujours disponible. (Cela dépend si une copie de cette vidéo est disponible). Ce type d’action est appelé un *temporaires* action.

Dans les métadonnées de service, une action temporaire a **IsAlwaysBindable** égal à false. C’est effectivement la valeur par défaut, et les métadonnées ressemblera à ceci :

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Voici pourquoi il est important : Si une action est temporaire, le serveur doit indiquer au client lorsque l’action est disponible. Pour ce faire, il comprend un lien vers l’action dans l’entité. Voici un exemple d’une entité de film :

[!code-console[Main](odata-actions/samples/sample17.cmd)]

La propriété « #CheckOut » contient un lien vers l’action d’extraction. Si l’action n’est pas disponible, le serveur omet le lien.

Pour déclarer une action temporaire dans le modèle EDM, appelez le **TransientAction** méthode :

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

En outre, vous devez fournir une fonction qui retourne un lien d’action pour une entité donnée. Définir cette fonction en appelant **HasActionLink**. Vous pouvez écrire la fonction comme une expression lambda :

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Si l’action est disponible, l’expression lambda renvoie un lien vers l’action. Le sérialiseur OData inclut ce lien lorsqu’il sérialise l’entité. Lorsque l’action n’est pas disponible, la fonction retourne `null`.

## <a name="additional-resources"></a>Ressources supplémentaires

[Exemple d’Actions OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
