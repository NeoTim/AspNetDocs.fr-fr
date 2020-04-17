---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Héritage de type complexe en OData v4 avec ASP.NET’API Web (fr) Microsoft Docs
author: rick-anderson
description: Selon les spécifications OData v4, un type complexe peut hériter d’un autre type complexe. (Un type complexe est un type structuré sans clé.) API Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: f433cf625c7d6ff4922d8c4a9954682fc0f1cc33
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543598"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Héritage de type complexe en OData v4 avec ASP.NET’API Web

par [Microsoft](https://github.com/microsoft)

> Selon les [spécifications](http://www.odata.org/documentation/odata-version-4-0/)OData v4 , un type complexe peut hériter d’un autre type complexe. (Un type *complexe* est un type structuré sans clé.) Web API OData 5.3 prend en charge l’héritage de type complexe.
> 
> Ce sujet montre comment construire un modèle de données d’entité (EDM) avec des types d’héritage complexes. Pour le code source complet, voir [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le tutoriel
> 
> 
> - Web API OData 5.3
> - OData v4

## <a name="model-hierarchy"></a>Hiérarchie du modèle

Pour illustrer l’héritage de type complexe, nous utiliserons la hiérarchie de classe suivante.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`est un type complexe abstrait. `Rectangle`, `Triangle`, `Circle` et sont `Shape`des `RoundRectangle` types complexes dérivés de , et dérive de `Rectangle`. `Window`est un type d’entité et contient une `Shape` instance.

Voici les classes CLR qui définissent ces types.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Construire le modèle EDM

Pour créer l’EDM, vous pouvez utiliser **ODataConventionModelBuilder**, qui déduit les relations d’héritage des types CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

Vous pouvez également construire l’EDM explicitement, en utilisant **ODataModelBuilder**. Cela prend plus de code, mais vous donne plus de contrôle sur l’EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

Ces deux exemples créent le même schéma EDM.

## <a name="metadata-document"></a>Document sur les métadonnées

Voici le document de métadonnées OData, montrant l’héritage de type complexe.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

D’après le document sur les métadonnées, vous pouvez voir que :

- Le `Shape` type complexe est abstrait.
- Le `Rectangle` `Triangle`type `Circle` , , et `Shape`complexe ont le type de base .
- Le `RoundRectangle` type a `Rectangle`le type de base .

## <a name="casting-complex-types"></a>Types complexes de casting

Le casting sur les types complexes est maintenant pris en charge. Par exemple, la requête suivante `Shape` jette `Rectangle`un .

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Voici la charge utile de réponse :

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
