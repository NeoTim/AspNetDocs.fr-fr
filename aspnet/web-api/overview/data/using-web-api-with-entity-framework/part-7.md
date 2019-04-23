---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Créer la vue (IU) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59408236"
---
# <a name="create-the-view-ui"></a>Créer la vue (Interface utilisateur)

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez démarrer définir le code HTML de l’application et ajouter une liaison de données entre le code HTML et le modèle de vue.

Ouvrez le fichier Views/Home/Index.cshtml. Remplacez tout le contenu de ce fichier avec les éléments suivants.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

La plupart de la `div` éléments existe-t-il pour [Bootstrap](http://getbootstrap.com/) styles. Les éléments importants sont ceux avec `data-bind` attributs. Cet attribut lie le code HTML au modèle de vue.

Exemple :

[!code-html[Main](part-7/samples/sample2.html)]

Dans cet exemple, le &quot; `text` &quot; liaison provoque la `<p>` élément pour afficher la valeur de la `error` propriété à partir du modèle de vue. N’oubliez pas que `error` a été déclaré comme un `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Chaque fois qu’une nouvelle valeur est affectée à `error`, Knockout met à jour le texte dans le `<p>` élément.

Le `foreach` liaison indique à Knockout pour itérer sur le contenu de la `books` tableau. Pour chaque élément du tableau, Knockout crée un &lt;li&gt; élément. Liaisons dans le contexte de la `foreach` font référence aux propriétés sur l’élément du tableau. Exemple :

[!code-html[Main](part-7/samples/sample4.html)]

Ici le `text` liaison lit la propriété de l’auteur de chaque ouvrage.

Si vous exécutez l’application maintenant, il doit ressembler à ceci :

![](part-7/_static/image1.png)

La liste des livres charge en mode asynchrone, une fois que la page se charge. Maintenant, le &quot;détails&quot; liens ne sont pas fonctionnels. Nous allons ajouter cette fonctionnalité dans la section suivante.

> [!div class="step-by-step"]
> [Précédent](part-6.md)
> [Suivant](part-8.md)
