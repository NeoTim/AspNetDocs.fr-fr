---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Création d’un extension de contrôle de la boîte à outils DE contrôle AJAX personnalisé (VB) Microsoft Docs
author: rick-anderson
description: Les extensions personnalisées vous permettent de personnaliser et d’étendre les capacités des contrôles ASP.NET sans avoir à créer de nouvelles classes.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: adce8e23ccf0a3364ee85534e98f8ea67ae4718d
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543676"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Création d’un extendeur de contrôle AJAX Control Toolkit personnalisé (VB)

par [Microsoft](https://github.com/microsoft)

> Les extensions personnalisées vous permettent de personnaliser et d’étendre les capacités des contrôles ASP.NET sans avoir à créer de nouvelles classes.

Dans ce tutoriel, vous apprenez à créer un extenseur de contrôle AJAX Control Toolkit personnalisé. Nous créons un nouvel extenseur simple, mais utile, qui modifie l’état d’un bouton d’un bouton désactivé à activé lorsque vous tapez du texte dans une TextBox. Après avoir lu ce tutoriel, vous serez en mesure d’étendre la ASP.NET boîte à outils AJAX avec vos propres extenseurs de contrôle.

Vous pouvez créer des extenseurs de contrôle personnalisés en utilisant Visual Studio ou Visual Web Developer (assurez-vous d’avoir la dernière version de Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Aperçu de l’extension DisabledButton

Notre nouvel extenseur de commande est nommé l’extenseur DisabledButton. Cet extenseur aura trois propriétés :

- TargetControlID - La TextBox que le contrôle étend.
- TargetButtonIID - Le bouton désactivé ou activé.
- DisabledText - Le texte qui est initialement affiché dans le bouton. Lorsque vous commencez à taper, le bouton affiche la valeur de la propriété Button Text.

Vous accrochez l’extenseur DisabledButton à un contrôle TextBox et Button. Avant de taper n’importe quel texte, le bouton est désactivé et la TextBox et le bouton ressemblent à ceci :

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Cliquez pour voir l’image pleine grandeur](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))

Après avoir commencé à taper du texte, le bouton est activé et la TextBox et le bouton ressemblent à ceci :

[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Cliquez pour voir l’image pleine grandeur](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))

Pour créer notre prolongateur de contrôle, nous devons créer les trois fichiers suivants :

- DisabledButtonExtender.vb - Ce fichier est la classe de contrôle côté serveur qui gérera la création de votre extenseur et vous permettra de définir les propriétés à l’heure de la conception. Il définit également les propriétés qui peuvent être définies sur votre extenseur. Ces propriétés sont accessibles via le code et à l’heure de conception et les propriétés de match définies dans le fichier DisableButtonBehavior.js.
- DisabledButtonBehavior.js -- Ce fichier est l’endroit où vous ajouterez toute la logique de script de votre client.
- DisabledButtonDesigner.vb - Cette classe permet la fonctionnalité design-temps. Vous avez besoin de cette classe si vous voulez que l’extension de contrôle fonctionne correctement avec le Visual Studio/Visual Web Developer Designer.

Ainsi, un extenseur de contrôle se compose d’un contrôle côté serveur, d’un comportement côté client et d’une classe de concepteur côté serveur. Vous apprenez à créer ces trois fichiers dans les sections suivantes.

## <a name="creating-the-custom-extender-website-and-project"></a>Création du site Web et du projet Custom Extender

La première étape consiste à créer un projet de bibliothèque de classe et un site Web dans Visual Studio/Visual Web Developer. Nous allons créer l’extension personnalisée dans le projet de bibliothèque de classe et de tester l’extension personnalisée dans le site Web.

Commençons par le site. Suivez ces étapes pour créer le site Web :

1. Sélectionnez l’option **menu Fichier, Nouveau site Web**.
2. Sélectionnez le **modèle ASP.NET site Web.**
3. Nommez le nouveau site *Web Website1*.
4. Cliquez sur le bouton **OK** .

Ensuite, nous devons créer le projet de bibliothèque de classe qui contiendra le code pour l’extension de contrôle:

1. Sélectionnez l’option **menu Fichier, Ajouter, Nouveau Projet**.
2. Sélectionnez le modèle **de bibliothèque de classe.**
3. Nommez la nouvelle bibliothèque de classe avec le nom **CustomExtenders**.
4. Cliquez sur le bouton **OK** .

Une fois que vous avez terminé ces étapes, votre fenêtre Solution Explorer devrait ressembler à la figure 1.

[![Solution avec le projet de bibliothèque de classe et de site Web](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figure 01**: Solution avec le site Web et le projet de bibliothèque de classe ([Cliquez pour voir l’image grandeur nature](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))

Ensuite, vous devez ajouter toutes les références d’assemblage nécessaires au projet de bibliothèque de classe :

1. Cliquez à droite sur le projet CustomExtenders et sélectionnez l’option de menu **Ajouter référence**.
2. Sélectionnez l'onglet .NET.
3. Ajoutez des références aux assemblys suivants :

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Sélectionnez l’onglet Parcourir.
5. Ajoutez une référence à l’assemblage AjaxControlToolkit.dll. Cet assemblage est situé dans le dossier où vous avez téléchargé la boîte à outils de contrôle AJAX.

Vous pouvez vérifier que vous avez ajouté toutes les bonnes références en cliquant à droite sur votre projet, en sélectionnant les propriétés et en cliquant sur l’onglet Références (voir la figure 2).

[![Dossier de références avec références requises](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figure 02**: Dossier de références avec références requises[(Cliquez pour voir l’image grandeur nature](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))

## <a name="creating-the-custom-control-extender"></a>Création de l’extension de contrôle personnalisé

Maintenant que nous avons notre bibliothèque de classe, nous pouvons commencer à construire notre contrôle d’extension. Commençons par les os nus d’une classe de contrôle d’extension personnalisée (voir Liste 1).

**Liste 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Il ya plusieurs choses que vous remarquez sur la classe d’extension de contrôle dans la liste 1. Tout d’abord, notez que la classe hérite de la classe de base ExtenderControlBase. Tous les contrôles d’extension AJAX Control Toolkit proviennent de cette classe de base. Par exemple, la classe de base comprend la propriété TargetID qui est une propriété requise de chaque extenseur de contrôle.

Ensuite, notez que la classe comprend les deux attributs suivants liés au script du client :

- WebResource - Fait en fait inclure un fichier comme ressource intégrée dans une assemblée.
- ClientScriptResource - Provoque la récupération d’une ressource de script à partir d’un assemblage.

L’attribut WebResource est utilisé pour intégrer le fichier JavaScript MyControlBehavior.js dans l’assemblage lorsque l’extenseur personnalisé est compilé. L’attribut ClientScriptResource est utilisé pour récupérer le script MyControlBehavior.js de l’assemblage lorsque l’extenseur personnalisé est utilisé dans une page Web.

Pour que les attributs WebResource et ClientScriptResource fonctionnent, vous devez compiler le fichier JavaScript comme une ressource intégrée. Sélectionnez le fichier dans la fenêtre Solution Explorer, ouvrez la feuille de propriété et attribuez la valeur *Embedded Resource* à la propriété **Build Action.**

Notez que l’extenseur de commande inclut également un attribut TargetControlType. Cet attribut est utilisé pour spécifier le type de contrôle qui est étendu par l’extenseur de commande. Dans le cas de la liste 1, l’extenseur de contrôle est utilisé pour étendre une TextBox.

Enfin, notez que l’extension personnalisée comprend une propriété nommée MyProperty. La propriété est marquée de l’attribut ExtenderControlProperty. Les méthodes GetPropertyValue() et SetPropertyValue() sont utilisées pour transmettre la valeur de la propriété de l’extenseur de contrôle côté serveur au comportement du côté du client.

Laissez-le aller de l’avant et implémentez le code de notre extenseur DisabledButton. Le code de cet extenseur peut être trouvé dans la liste 2.

**Liste 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

L’extension DisabledButton dans La liste 2 a deux propriétés nommées TargetButtonID et DisabledText. L’IDReferenceProperty appliqué à la propriété TargetButtonID vous empêche d’attribuer autre chose que l’ID d’un contrôle bouton à cette propriété.

Les attributs WebResource et ClientScriptResource associent un comportement côté client situé dans un fichier nommé DisabledButtonBehavior.js avec cet extenseur. Nous discutons de ce fichier JavaScript dans la section suivante.

## <a name="creating-the-custom-extender-behavior"></a>Création du comportement d’extension personnalisé

Le composant du côté client d’un extenseur de contrôle est appelé un comportement. La logique réelle pour désactiver et permettre le bouton est contenue dans le comportement DisabledButton. Le code JavaScript pour le comportement est inclus dans la liste 3.

**Liste 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

Le fichier JavaScript dans La liste 3 contient une classe côté client nommée DisabledButtonBehavior. Cette classe, comme son jumeau côté serveur, comprend deux propriétés nommées TargetButtonID et\_DisabledText auxquelles vous\_pouvez accéder en utilisant get\_TargetButtonID/set TargetButtonID et obtenir DisabledText/set\_DisabledText.

La méthode initialiser () associe un gestionnaire d’événements keyup à l’élément cible du comportement. Chaque fois que vous tapez une lettre dans la TextBox associée à ce comportement, le gestionnaire de keyup exécute. Le gestionnaire de keyup permet ou désactive le bouton selon que la TextBox associée au comportement contient n’importe quel texte.

N’oubliez pas que vous devez compiler le fichier JavaScript dans la liste 3 comme une ressource intégrée. Sélectionnez le fichier dans la fenêtre Solution Explorer, ouvrez la feuille de propriété et attribuez la valeur *embedded Resource* à la propriété **Build Action** (voir la figure 3). Cette option est disponible en Visual Studio et Visual Web Developer.

[![Ajout d’un fichier JavaScript comme ressource intégrée](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figure 03**: Ajout d’un fichier JavaScript comme ressource intégrée[(Cliquez pour voir l’image grandeur nature](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))

## <a name="creating-the-custom-extender-designer"></a>Création du concepteur d’extensions personnalisées

Il y a une dernière classe que nous devons créer pour compléter notre prolongateur. Nous devons créer la classe de concepteur dans la liste 4. Cette classe est nécessaire pour que l’extenseur se comporte correctement avec le Visual Studio/Visual Web Developer Designer.

**Liste 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Vous associez le concepteur dans La liste 4 avec l’extension DisabledButton avec l’attribut Designer. Vous devez appliquer l’attribut Designer à la classe DisabledButtonExtender comme ceci:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Utilisation de l’extension personnalisée

Maintenant que nous avons fini de créer l’extension de contrôle DisabledButton, il est temps de l’utiliser dans notre site Web ASP.NET. Tout d’abord, nous devons ajouter l’extenseur personnalisé à la boîte à outils. Procédez comme suit :

1. Ouvrez une page ASP.NET en cliquant doublement sur la page dans la fenêtre Solution Explorer.
2. Cliquez à droite sur la boîte à outils et sélectionnez l’option menu **Choisissez des éléments**.
3. Dans le dialogue Choisissez les articles de boîte à outils, naviguez sur l’assemblage CustomExtenders.dll.
4. Cliquez sur le bouton **OK** pour fermer le dialogue.

Une fois que vous avez terminé ces étapes, l’extenseur de commande DisabledButton doit apparaître dans la boîte à outils (voir la figure 4).

[![DisabledButton dans la boîte à outils](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figure 04**: DisabledButton dans la boîte à outils[(Cliquez pour voir l’image grandeur nature](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))

Ensuite, nous devons créer une nouvelle page ASP.NET. Procédez comme suit :

1. Créez une nouvelle page ASP.NET nommée ShowDisabledButton.aspx.
2. Faites glisser un ScriptManager sur la page.
3. Faites glisser un contrôle TextBox sur la page.
4. Faites glisser un contrôle de bouton sur la page.
5. Dans la fenêtre Propriétés, modifier la propriété Button ID à la valeur <em>btnSave</em> et la propriété Text à la valeur *Enregistrer\**.

Nous avons créé une page avec un contrôle standard ASP.NET TextBox et Button.

Ensuite, nous devons étendre le contrôle TextBox avec l’extension DisabledButton:

1. Sélectionnez l’option **Add Extender** task pour ouvrir le dialogue Extender Wizard (voir la figure 5). Notez que le dialogue comprend notre extenseur DisabledButton personnalisé.
2. Sélectionnez l’extenseur DisabledButton et cliquez sur le bouton **OK.**

[![Le dialogue De l’Extender Wizard](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figure 05**: Le dialogue De l’Extender Wizard[(Cliquez pour voir l’image grandeur nature](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))

Enfin, nous pouvons définir les propriétés de l’extenseur DisabledButton. Vous pouvez modifier les propriétés de l’extenseur DisabledButton en modifiant les propriétés du contrôle TextBox :

1. Sélectionnez la TextBox dans le concepteur.
2. Dans la fenêtre Propriétés, étendre le nœud Extenders (voir la figure 6).
3. Attribuez la valeur *Enregistrer* à la propriété DisabledText et la valeur *btnSave* à la propriété TargetButtonID.

[![Définir les propriétés de l’extension](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figure 06**: Réglage des propriétés d’extenseur ([Cliquez pour voir l’image grandeur nature](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))

Lorsque vous exécutez la page (en frappant F5), le contrôle Button est initialement désactivé. Dès que vous commencez à entrer du texte dans la TextBox, le contrôle du bouton est activé (voir la figure 7).

[![L’extension DisabledButton en action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figure 07**: L’extenseur DisabledButton en action[(Cliquez pour voir l’image grandeur nature](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))

## <a name="summary"></a>Récapitulatif

Le but de ce tutoriel était d’expliquer comment vous pouvez étendre la boîte à outils de contrôle AJAX avec des commandes d’extension personnalisées. Dans ce tutoriel, nous avons créé un simple prolongateur de contrôle DisabledButton. Nous avons mis en œuvre cet extension en créant une classe DisabledButtonExtender, un comportement JavaScript DisabledButtonBehavior, et une classe DisabledButtonDesigner. Vous suivez un ensemble similaire d’étapes chaque fois que vous créez un prolongateur de contrôle personnalisé.

> [!div class="step-by-step"]
> [Précédent](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
