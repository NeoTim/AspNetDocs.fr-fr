---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Ajouter un nouvel élément à la base de données | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 01586ef6eecdbe1acfadfcfcc0c9ca8b442de2d3
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57045026"
---
<a name="add-a-new-item-to-the-database"></a>Ajouter un nouvel élément à la base de données
====================
par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez ajouter la possibilité aux utilisateurs de créer un nouveau livre. Dans app.js, ajoutez le code suivant au modèle de vue :

[!code-javascript[Main](part-9/samples/sample1.js)]

Dans Index.cshtml, remplacez le balisage suivant :

[!code-html[Main](part-9/samples/sample2.html)]

Par :

[!code-html[Main](part-9/samples/sample3.html)]

Ce balisage crée un formulaire pour l’envoi d’un auteur de nouveau. Les valeurs de la liste déroulante auteur sont liés aux données à le `authors` observable dans le modèle de vue. Pour les autres entrées de formulaire, les valeurs sont liés aux données à le `newBook` propriété du modèle de vue.

Le Gestionnaire d’envoi du formulaire est lié à la `addBook` (fonction) :

[!code-html[Main](part-9/samples/sample4.html)]

Le `addBook` fonction lit les valeurs actuelles des entrées de formulaire lié aux données pour créer un objet JSON. Puis il publie l’objet JSON à `/api/books`.

> [!div class="step-by-step"]
> [Précédent](part-8.md)
> [Suivant](part-10.md)
