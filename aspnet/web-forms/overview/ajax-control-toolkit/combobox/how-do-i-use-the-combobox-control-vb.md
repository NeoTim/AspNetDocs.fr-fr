---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Comment utiliser le contrôle de zone de liste déroulante ? (VB) | Microsoft Docs
author: microsoft
description: Zone de liste déroulante est un contrôle ASP.NET AJAX qui combine la souplesse d’une zone de texte avec une liste d’options à partir de laquelle les utilisateurs peuvent choisir.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ec00a58581f36f87ecdca2fbd96fcea645f75f48
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030916"
---
<a name="how-do-i-use-the-combobox-control-vb"></a><span data-ttu-id="56160-104">Comment utiliser le contrôle de zone de liste déroulante ?</span><span class="sxs-lookup"><span data-stu-id="56160-104">How do I use the ComboBox Control?</span></span> <span data-ttu-id="56160-105">(VB)</span><span class="sxs-lookup"><span data-stu-id="56160-105">(VB)</span></span>
====================
<span data-ttu-id="56160-106">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="56160-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="56160-107">Zone de liste déroulante est un contrôle ASP.NET AJAX qui combine la souplesse d’une zone de texte avec une liste d’options à partir de laquelle les utilisateurs peuvent choisir.</span><span class="sxs-lookup"><span data-stu-id="56160-107">ComboBox is an ASP.NET AJAX control that combines the flexibility of a TextBox with a list of options from which users can choose.</span></span>


<span data-ttu-id="56160-108">L’objectif de ce didacticiel est d’expliquer le contrôle ComboBox de boîte à outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="56160-108">The goal of this tutorial is to explain the AJAX Control Toolkit ComboBox control.</span></span> <span data-ttu-id="56160-109">La zone de liste déroulante fonctionne comme une combinaison entre un contrôle DropDownList de ASP.NET standard et un contrôle de zone de texte.</span><span class="sxs-lookup"><span data-stu-id="56160-109">The ComboBox works like a combination between a standard ASP.NET DropDownList control and a TextBox control.</span></span> <span data-ttu-id="56160-110">Vous pouvez sélectionner dans une liste déjà existante d’éléments ou entrez un nouvel élément.</span><span class="sxs-lookup"><span data-stu-id="56160-110">You can either select from a pre-existing list of items or enter a new item.</span></span>

<span data-ttu-id="56160-111">La zone de liste déroulante est similaire à l’extendeur de contrôle de la saisie semi-automatique, mais les contrôles sont utilisés dans des scénarios différents.</span><span class="sxs-lookup"><span data-stu-id="56160-111">The ComboBox is similar to the AutoComplete control extender, but the controls are used in different scenarios.</span></span> <span data-ttu-id="56160-112">L’extendeur AutoComplete interroge un service web pour obtenir des entrées correspondantes.</span><span class="sxs-lookup"><span data-stu-id="56160-112">The AutoComplete extender queries a web service to get matching entries.</span></span> <span data-ttu-id="56160-113">Le contrôle de zone de liste déroulante, en revanche, est initialisé avec un ensemble d’éléments.</span><span class="sxs-lookup"><span data-stu-id="56160-113">The ComboBox control, in contrast, is initialized with a set of items.</span></span> <span data-ttu-id="56160-114">À l’aide de la rend extendeur AutoComplete sens lorsque vous travaillez avec un grand nombre de données (des millions de pièces de voiture) tout en utilisant le contrôle de zone de liste déroulante de sens lorsque vous travaillez avec un petit ensemble de données (des dizaines de parties de la voiture).</span><span class="sxs-lookup"><span data-stu-id="56160-114">Using the AutoComplete extender makes sense when you are working with a large set of data (millions of car parts) while using the ComboBox control makes sense when working with a small set of data (dozens of car parts).</span></span>

## <a name="selecting-from-a-static-list-of-items"></a><span data-ttu-id="56160-115">Sélection d’une liste statique d’éléments</span><span class="sxs-lookup"><span data-stu-id="56160-115">Selecting from a Static List of Items</span></span>

<span data-ttu-id="56160-116">Laisser s démarrer avec un exemple simple d’utilisation du contrôle de zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-116">Let�s start with a simple sample of using the ComboBox control.</span></span> <span data-ttu-id="56160-117">Imaginez que vous souhaitez afficher une liste statique d’éléments dans une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-117">Imagine that you want to display a static list of items in a dropdown list.</span></span> <span data-ttu-id="56160-118">Toutefois, vous souhaitez laissez ouverte la possibilité que la liste n’est pas terminée.</span><span class="sxs-lookup"><span data-stu-id="56160-118">However, you want to leave open the possibility that the list is not complete.</span></span> <span data-ttu-id="56160-119">Vous souhaitez autoriser un utilisateur à entrer une valeur personnalisée dans la liste.</span><span class="sxs-lookup"><span data-stu-id="56160-119">You want to allow a user to enter a custom value into the list.</span></span>

<span data-ttu-id="56160-120">Nous ll créer une page Web Forms ASP.NET et utilisez le contrôle de zone de liste déroulante de la page.</span><span class="sxs-lookup"><span data-stu-id="56160-120">We�ll create a new ASP.NET Web Forms page and use the ComboBox control in the page.</span></span> <span data-ttu-id="56160-121">Ajouter la nouvelle page ASP.NET à votre projet et basculez en mode Design.</span><span class="sxs-lookup"><span data-stu-id="56160-121">Add the new ASP.NET page to your project and switch to Design view.</span></span>

<span data-ttu-id="56160-122">Si vous souhaitez utiliser le contrôle de zone de liste déroulante dans la page vous devez ajouter un contrôle ScriptManager à la page.</span><span class="sxs-lookup"><span data-stu-id="56160-122">If you want to use the ComboBox control in the page then you must add a ScriptManager control to the page.</span></span> <span data-ttu-id="56160-123">Faites glisser le contrôle ScriptManager sous l’onglet Extensions AJAX sur l’aire du concepteur.</span><span class="sxs-lookup"><span data-stu-id="56160-123">Drag the ScriptManager control from beneath the AJAX Extensions tab onto the Designer surface.</span></span> <span data-ttu-id="56160-124">Vous devez ajouter le contrôle ScriptManager en haut de la page ; Vous pouvez l’ajouter immédiatement au-dessous de l’ouverture côté serveur &lt;formulaire&gt; balise.</span><span class="sxs-lookup"><span data-stu-id="56160-124">You should add the ScriptManager control at the top of the page; you can add it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="56160-125">Ensuite, faites glisser le contrôle de zone de liste déroulante sur la page.</span><span class="sxs-lookup"><span data-stu-id="56160-125">Next, drag the ComboBox control onto the page.</span></span> <span data-ttu-id="56160-126">Vous trouverez le contrôle de zone de liste déroulante dans la boîte à outils avec les autres contrôles AJAX Control Toolkit et extendeurs de contrôle (voir figure 1).</span><span class="sxs-lookup"><span data-stu-id="56160-126">You can find the ComboBox control in the Toolbox with the other AJAX Control Toolkit controls and control extenders (see figure1).</span></span>


<span data-ttu-id="56160-127">[![Formulaire simple pour la création d’une carte de visite](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="56160-127">[![Simple form for creating a business card](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="56160-128">**Figure 01**: Sélection du contrôle de zone de liste déroulante à partir de la boîte à outils ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="56160-128">**Figure 01**: Selecting the ComboBox control from the toolbox ([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span></span>


<span data-ttu-id="56160-129">Nous ll utiliser le contrôle de zone de liste déroulante pour afficher une liste statique de choix.</span><span class="sxs-lookup"><span data-stu-id="56160-129">We�ll use the ComboBox control to display a static list of choices.</span></span> <span data-ttu-id="56160-130">L’utilisateur peut sélectionner un niveau particulier de spiciness pour leurs produits alimentaires à partir d’une liste de choix de trois : Violence, support et à chaud (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="56160-130">The user can select a particular level of spiciness for their food from a list of three choices: Mild, Medium, and Hot (see Figure 2).</span></span>


<span data-ttu-id="56160-131">[![Sélection d’une liste statique d’éléments](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="56160-131">[![Selecting from a static list of items](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span></span>

<span data-ttu-id="56160-132">**Figure 02**: Sélection d’une liste statique d’éléments ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="56160-132">**Figure 02**: Selecting from a static list of items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span></span>


<span data-ttu-id="56160-133">Il existe deux façons que vous pouvez ajouter ces choix pour le contrôle de zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-133">There are two ways that you can add these choices to the ComboBox control.</span></span> <span data-ttu-id="56160-134">Tout d’abord, vous sélectionnez l’option de tâche de modifier les Options lorsque vous pointez votre souris sur le contrôle en mode Design et que vous ouvrez l’éditeur d’élément (voir Figure 3).</span><span class="sxs-lookup"><span data-stu-id="56160-134">First, you select the Edit Options task option when hovering your mouse over the control in Design view and open the Item Editor (see Figure 3).</span></span>


<span data-ttu-id="56160-135">[![Modification d’éléments de la zone de liste déroulante](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="56160-135">[![Editing ComboBox items](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span></span>

<span data-ttu-id="56160-136">**Figure 03**: Modification d’éléments de la zone de liste déroulante ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="56160-136">**Figure 03**: Editing ComboBox items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span></span>


<span data-ttu-id="56160-137">La deuxième option consiste à ajouter la liste des éléments entre les balises &lt;asp : zone de liste déroulante&gt; balises en mode Source.</span><span class="sxs-lookup"><span data-stu-id="56160-137">The second option is to add the list of items in between the opening and closing &lt;asp:ComboBox&gt; tags in Source view.</span></span> <span data-ttu-id="56160-138">La page dans le Listing 1 contient la zone de liste déroulante mis à jour comportant la liste d’éléments.</span><span class="sxs-lookup"><span data-stu-id="56160-138">The page in Listing 1 contains the updated ComboBox that has the list of items.</span></span>

<span data-ttu-id="56160-139">**Liste 1 - Static.aspx**</span><span class="sxs-lookup"><span data-stu-id="56160-139">**Listing 1 - Static.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

<span data-ttu-id="56160-140">Lorsque vous ouvrez la page dans le Listing 1, vous pouvez sélectionner une des options existantes à partir de la zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-140">When you open the page in Listing 1, you can select one of the pre-existing options from the ComboBox.</span></span> <span data-ttu-id="56160-141">En d’autres termes, la zone de liste déroulante fonctionne exactement comme un contrôle DropDownList.</span><span class="sxs-lookup"><span data-stu-id="56160-141">In other words, the ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="56160-142">Toutefois, vous avez également la possibilité d’entrer un nouveau choix (par exemple, Super corsée) qui n’est pas dans la liste existante.</span><span class="sxs-lookup"><span data-stu-id="56160-142">However, you also have the option of entering a new choice (for example, Super Spicy) that is not in the existing list.</span></span> <span data-ttu-id="56160-143">Par conséquent, la zone de liste déroulante fonctionne également comme un contrôle de zone de texte.</span><span class="sxs-lookup"><span data-stu-id="56160-143">So, the ComboBox also works like a TextBox control.</span></span>

<span data-ttu-id="56160-144">Indépendamment de si vous choisissez un préexistants élément ou que vous entrez un élément personnalisé, lorsque vous envoyez le formulaire, votre choix s’affiche dans le contrôle d’étiquette.</span><span class="sxs-lookup"><span data-stu-id="56160-144">Regardless of whether you pick a pre-existing item or you enter a custom item, when you submit the form, your choice appears in the label control.</span></span> <span data-ttu-id="56160-145">Lorsque vous envoyez le formulaire, le niveau de btnSubmit\_cliquez sur Gestionnaire s’exécute et met à jour de l’étiquette (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="56160-145">When you submit the form, the btnSubmit\_Click handler executes and updates the label (see Figure 4).</span></span>


<span data-ttu-id="56160-146">[![Affichage de l’élément sélectionné](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="56160-146">[![Displaying the selected item](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span></span>

<span data-ttu-id="56160-147">**Figure 04**: Affichage de l’élément sélectionné ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="56160-147">**Figure 04**: Displaying the selected item([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span></span>


<span data-ttu-id="56160-148">La zone de liste déroulante prend en charge les mêmes propriétés que le contrôle DropDownList pour la récupération de l’élément sélectionné après l’envoi d’un formulaire :</span><span class="sxs-lookup"><span data-stu-id="56160-148">The ComboBox supports the same properties as the DropDownList control for retrieving the selected item after a form is submitted:</span></span>

- <span data-ttu-id="56160-149">SelectedItem.Text - affiche la valeur de la propriété Text de l’élément sélectionné.</span><span class="sxs-lookup"><span data-stu-id="56160-149">SelectedItem.Text - Displays the value of the Text property of the selected item.</span></span>
- <span data-ttu-id="56160-150">SelectedItem.Value - affiche la valeur de la propriété Value de l’élément sélectionné ou affiche le texte tapé dans la zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-150">SelectedItem.Value - Displays the value of the Value property of the selected item or displays the text typed into the ComboBox.</span></span>
- <span data-ttu-id="56160-151">SelectedValue - même en tant que SelectedItem.Value, à ceci près que cette propriété vous permet de spécifier l’élément sélectionné (initiale) par défaut.</span><span class="sxs-lookup"><span data-stu-id="56160-151">SelectedValue - Same as SelectedItem.Value except that this property enables you to specify the default (initial) selected item.</span></span>

<span data-ttu-id="56160-152">Si vous tapez un choix personnalisé dans la zone de liste déroulante puis le choix personnalisé est affecté aux propriétés SelectedItem.Text et de SelectedItem.Value.</span><span class="sxs-lookup"><span data-stu-id="56160-152">If you type a custom choice into the ComboBox then the custom choice is assigned to both the SelectedItem.Text and SelectedItem.Value properties.</span></span>

## <a name="selecting-the-list-of-items-from-the-database"></a><span data-ttu-id="56160-153">En sélectionnant la liste des éléments à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="56160-153">Selecting the List of Items from the Database</span></span>

<span data-ttu-id="56160-154">Vous pouvez récupérer la liste des éléments qui affiche la zone de liste déroulante à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="56160-154">You can retrieve the list of items that the ComboBox displays from a database.</span></span> <span data-ttu-id="56160-155">Par exemple, vous pouvez lier la zone de liste déroulante pour un contrôle SqlDataSource, un contrôle ObjectDataSource, un LinqDataSource ou un contrôle EntityDataSource.</span><span class="sxs-lookup"><span data-stu-id="56160-155">For example, you can bind the ComboBox to a SqlDataSource control, an ObjectDataSource control, a LinqDataSource, or an EntityDataSource.</span></span>

<span data-ttu-id="56160-156">Imaginez que vous souhaitez afficher une liste de films dans une zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-156">Imagine that you want to display a list of movies in a ComboBox.</span></span> <span data-ttu-id="56160-157">Vous souhaitez récupérer la liste de films à partir de la table de base de données de films.</span><span class="sxs-lookup"><span data-stu-id="56160-157">You want to retrieve the list of movies from the Movies database table.</span></span> <span data-ttu-id="56160-158">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="56160-158">Follow these steps:</span></span>

1. <span data-ttu-id="56160-159">Créez une page nommée Movies.aspx</span><span class="sxs-lookup"><span data-stu-id="56160-159">Create a page named Movies.aspx</span></span>
2. <span data-ttu-id="56160-160">Ajouter un contrôle ScriptManager à la page en faisant glisser le ScriptManager sous l’onglet Extensions AJAX dans la boîte à outils vers la page.</span><span class="sxs-lookup"><span data-stu-id="56160-160">Add a ScriptManager control to the page by dragging the ScriptManager from under the AJAX Extensions tab in the Toolbox onto the page.</span></span>
3. <span data-ttu-id="56160-161">Ajouter un contrôle de zone de liste déroulante à la page en faisant glisser de la zone de liste déroulante sur la page.</span><span class="sxs-lookup"><span data-stu-id="56160-161">Add a ComboBox control to the page by dragging the ComboBox onto the page.</span></span>
4. <span data-ttu-id="56160-162">En mode conception, pointez votre souris sur le contrôle de zone de liste déroulante et sélectionnez le **choisir la Source de données** option de tâche (voir Figure 5).</span><span class="sxs-lookup"><span data-stu-id="56160-162">In Design view, hover your mouse over the ComboBox control and select the **Choose Data Source** task option (see Figure 5).</span></span> <span data-ttu-id="56160-163">L’Assistant de Configuration de Source de données est lancé.</span><span class="sxs-lookup"><span data-stu-id="56160-163">The Data Source Configuration Wizard is launched.</span></span>
5. <span data-ttu-id="56160-164">Dans le **choisir une Source de données** étape, sélectionnez le &lt;nouvelle source de données&gt; option.</span><span class="sxs-lookup"><span data-stu-id="56160-164">In the **Choose a Data Source** step, select the &lt;New data source�&gt; option.</span></span>
6. <span data-ttu-id="56160-165">Dans le **choisir un Type de Source de données** étape, sélectionnez la base de données.</span><span class="sxs-lookup"><span data-stu-id="56160-165">In the **Choose a Data Source Type** step, select Database.</span></span>
7. <span data-ttu-id="56160-166">Dans le **choisir votre connexion de données** étape, sélectionnez votre base de données (par exemple, MoviesDB.mdf).</span><span class="sxs-lookup"><span data-stu-id="56160-166">In the **Choose Your Data Connection** step, select your database (for example, MoviesDB.mdf).</span></span>
8. <span data-ttu-id="56160-167">Dans le **enregistrer la chaîne de connexion au fichier de Configuration de l’Application** étape, sélectionnez l’option pour enregistrer votre chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="56160-167">In the **Save the Connection String to the Application Configuration File** step, select the option to save your connection string.</span></span>
9. <span data-ttu-id="56160-168">Dans le **configurer l’instruction Select** étape, sélectionnez la table de base de données de films et sélectionnez toutes les colonnes.</span><span class="sxs-lookup"><span data-stu-id="56160-168">In the **Configure the Select Statement** step, select the Movies database table and select all of the columns.</span></span>
10. <span data-ttu-id="56160-169">Dans le **tester la requête** étape, cliquez sur le bouton Terminer.</span><span class="sxs-lookup"><span data-stu-id="56160-169">In the **Test Query** step, click the Finish button.</span></span>
11. <span data-ttu-id="56160-170">Dans le **choisir la Source de données** étape, sélectionnez la colonne de titre pour le champ à afficher et de la colonne Id pour les données de champ (voir Figure).</span><span class="sxs-lookup"><span data-stu-id="56160-170">Back in the **Choose Data Source** step, select the Title column for the field to display and the Id column for the data field (see Figure).</span></span>
12. <span data-ttu-id="56160-171">Cliquez sur le bouton OK pour fermer l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="56160-171">Click the OK button to close the wizard.</span></span>


<span data-ttu-id="56160-172">[![Choix d’une source de données](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="56160-172">[![Choosing a data source](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span></span>

<span data-ttu-id="56160-173">**Figure 05**: Choix d’une source de données ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="56160-173">**Figure 05**: Choosing a data source([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span></span>


<span data-ttu-id="56160-174">[![Choisir les champs de texte et la valeur de données](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="56160-174">[![Choosing the data text and value fields](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span></span>

<span data-ttu-id="56160-175">**Figure 06**: Choisir les champs de texte et la valeur de données ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="56160-175">**Figure 06**: Choosing the data text and value fields([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span></span>


<span data-ttu-id="56160-176">Après avoir terminé les étapes ci-dessus, la zone de liste déroulante est liée à un contrôle SqlDataSource qui représente les films à partir de la table de base de données de films.</span><span class="sxs-lookup"><span data-stu-id="56160-176">After you complete the steps above, the ComboBox is bound to a SqlDataSource control that represents the movies from the Movies database table.</span></span> <span data-ttu-id="56160-177">La source de la page ressemble à la liste 2 (j’ai nettoyé un peu la mise en forme).</span><span class="sxs-lookup"><span data-stu-id="56160-177">The source for the page looks like Listing 2 (I cleaned up the formatting a little bit).</span></span>

<span data-ttu-id="56160-178">**Listing 2 - Movies.aspx**</span><span class="sxs-lookup"><span data-stu-id="56160-178">**Listing 2 - Movies.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

<span data-ttu-id="56160-179">Notez que le contrôle de zone de liste déroulante a une propriété DataSourceID qui pointe vers le contrôle SqlDataSource.</span><span class="sxs-lookup"><span data-stu-id="56160-179">Notice that the ComboBox control has a DataSourceID property that points to the SqlDataSource control.</span></span> <span data-ttu-id="56160-180">Lorsque vous ouvrez la page dans un navigateur, la liste de films à partir de la base de données s’affiche (voir la Figure 7).</span><span class="sxs-lookup"><span data-stu-id="56160-180">When you open the page in a browser, the list of movies from the database is displayed (see Figure 7).</span></span> <span data-ttu-id="56160-181">Vous pouvez un choix un film à partir de la liste ou entrez un nouveau film en tapant le film dans la zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-181">You can either a pick a movie from the list or enter a new movie by typing the movie into the ComboBox.</span></span>


<span data-ttu-id="56160-182">[![Affiche une liste de films](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="56160-182">[![Displaying a list of movies](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span></span>

<span data-ttu-id="56160-183">**Figure 07**: Affiche une liste de films ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="56160-183">**Figure 07**: Displaying a list of movies([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span></span>


## <a name="setting-the-dropdownstyle"></a><span data-ttu-id="56160-184">Définissant le DropDownStyle</span><span class="sxs-lookup"><span data-stu-id="56160-184">Setting the DropDownStyle</span></span>

<span data-ttu-id="56160-185">Vous pouvez utiliser la propriété ComboBox DropDownStyle pour modifier le comportement de la zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-185">You can use the ComboBox DropDownStyle property to change the behavior of the ComboBox.</span></span> <span data-ttu-id="56160-186">Cette propriété accepte il les valeurs possibles :</span><span class="sxs-lookup"><span data-stu-id="56160-186">This property accepts there possible values:</span></span>

- <span data-ttu-id="56160-187">Liste déroulante - affiche de la zone de liste déroulante (valeur par défaut) une liste déroulante liste lorsque vous cliquez sur la flèche et vous pouvez entrer une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="56160-187">DropDown - (default value) The ComboBox displays a dropdown list when you click the arrow and you can enter a custom value.</span></span>
- <span data-ttu-id="56160-188">Simple - la zone de liste déroulante s’affiche automatiquement une liste déroulante et vous pouvez entrer une valeur personnalisée.</span><span class="sxs-lookup"><span data-stu-id="56160-188">Simple - The ComboBox displays a dropdown list automatically and you can enter a custom value.</span></span>
- <span data-ttu-id="56160-189">DropDownList - la zone de liste déroulante fonctionne exactement comme un contrôle DropDownList.</span><span class="sxs-lookup"><span data-stu-id="56160-189">DropDownList - The ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="56160-190">Les différentes entre la liste déroulante et Simple sont lorsque la liste des éléments est affichée.</span><span class="sxs-lookup"><span data-stu-id="56160-190">The different between DropDown and Simple is when the list of items is displayed.</span></span> <span data-ttu-id="56160-191">Dans le cas Simple, la liste s’affiche immédiatement lorsque vous déplacez le focus vers la zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-191">In the case of Simple, the list is displayed immediately when you move focus to the ComboBox.</span></span> <span data-ttu-id="56160-192">Dans le cas de liste déroulante, vous devez cliquer sur la flèche pour afficher la liste des éléments.</span><span class="sxs-lookup"><span data-stu-id="56160-192">In the case of DropDown, you must click the arrow to see the list of items.</span></span>

<span data-ttu-id="56160-193">La valeur de DropDownList, le contrôle de zone de liste déroulante de fonctionner comme un contrôle DropDownList standard.</span><span class="sxs-lookup"><span data-stu-id="56160-193">The DropDownList value causes the ComboBox control to work just like a standard DropDownList control.</span></span> <span data-ttu-id="56160-194">Toutefois, il existe une importante différence ici.</span><span class="sxs-lookup"><span data-stu-id="56160-194">However, there is an important difference here.</span></span> <span data-ttu-id="56160-195">Les versions antérieures d’Internet Explorer affichent un contrôle DropDownList avec un z-index infini pour le contrôle s’affiche devant n’importe quel contrôle placé devant elle.</span><span class="sxs-lookup"><span data-stu-id="56160-195">Older versions of Internet Explorer display a DropDownList control with an infinite z-index so the control will appear in front of any control placed in front of it.</span></span> <span data-ttu-id="56160-196">Étant donné que le contrôle ComboBox effectue le rendu HTML &lt;div&gt; balise au lieu d’un élément HTML &lt;sélectionnez&gt; balise, le contrôle ComboBox correctement respecte positionnement z.</span><span class="sxs-lookup"><span data-stu-id="56160-196">Because the ComboBox renders an HTML &lt;div&gt; tag instead of an HTML &lt;select&gt; tag, the ComboBox correctly respects z-ordering.</span></span>

## <a name="setting-the-autocompletemode"></a><span data-ttu-id="56160-197">Définissant le AutoCompleteMode</span><span class="sxs-lookup"><span data-stu-id="56160-197">Setting the AutoCompleteMode</span></span>

<span data-ttu-id="56160-198">La propriété ComboBox AutoCompleteMode vous permet de spécifier que se passe-t-il quand un utilisateur tape du texte dans la zone de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="56160-198">You use the ComboBox AutoCompleteMode property to specify what happens when someone types text into the ComboBox.</span></span> <span data-ttu-id="56160-199">Cette propriété accepte les valeurs possibles suivantes :</span><span class="sxs-lookup"><span data-stu-id="56160-199">This property accepts the following possible values:</span></span>

- <span data-ttu-id="56160-200">None : (valeur par défaut) la zone de liste déroulante ne fournit pas de comportement de saisie semi-automatique.</span><span class="sxs-lookup"><span data-stu-id="56160-200">None - (default value) The ComboBox does not provide any auto-complete behavior.</span></span>
- <span data-ttu-id="56160-201">Suggérer - la zone de liste déroulante affiche la liste et il met en surbrillance l’élément correspondant dans la liste (voir Figure 8).</span><span class="sxs-lookup"><span data-stu-id="56160-201">Suggest - The ComboBox displays the list and it highlights the matching item in the list (see Figure 8).</span></span>
- <span data-ttu-id="56160-202">Append : le contrôle ComboBox n’affiche pas la liste, et il ajoute l’élément correspondant dans la liste sur ce que vous avez tapé (voir Figure 9).</span><span class="sxs-lookup"><span data-stu-id="56160-202">Append - The ComboBox does not display the list and it appends the matching item from the list onto what you have typed (see Figure 9).</span></span>
- <span data-ttu-id="56160-203">À la fois SuggestAppend - la zone de liste déroulante affiche la liste et ajoute l’élément correspondant dans la liste sur ce que vous avez tapé (voir Figure 10).</span><span class="sxs-lookup"><span data-stu-id="56160-203">SuggestAppend - The ComboBox both displays the list and appends the matching item from the list onto what you have typed (see Figure 10).</span></span>


<span data-ttu-id="56160-204">[![La zone de liste déroulante affiche une suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="56160-204">[![The ComboBox makes a suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span></span>

<span data-ttu-id="56160-205">**Figure 08**: La zone de liste déroulante affiche une suggestion ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="56160-205">**Figure 08**: The ComboBox makes a suggestion([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span></span>


<span data-ttu-id="56160-206">[![Zone de liste déroulante ajoute du texte correspondant](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="56160-206">[![ComboBox appends matching text](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span></span>

<span data-ttu-id="56160-207">**Figure 09**: Zone de liste déroulante ajoute du texte correspondant ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="56160-207">**Figure 09**: ComboBox appends matching text([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span></span>


<span data-ttu-id="56160-208">[![La zone de liste déroulante suggère et ajoute](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="56160-208">[![The ComboBox suggests and appends](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span></span>

<span data-ttu-id="56160-209">**Figure 10**: La zone de liste déroulante suggère et ajoute ([cliquez pour afficher l’image en taille réelle](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="56160-209">**Figure 10**: The ComboBox suggests and appends([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span></span>


## <a name="summary"></a><span data-ttu-id="56160-210">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="56160-210">Summary</span></span>

<span data-ttu-id="56160-211">Dans ce didacticiel, vous avez appris à utiliser le contrôle de zone de liste déroulante pour afficher un ensemble fixe d’éléments.</span><span class="sxs-lookup"><span data-stu-id="56160-211">In this tutorial, you learned how to use the ComboBox control to display a fixed set of items.</span></span> <span data-ttu-id="56160-212">Nous lié le contrôle de zone de liste déroulante à la fois à un ensemble d’éléments de statique et à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="56160-212">We bound the ComboBox control both to a static set of items and to a database table.</span></span> <span data-ttu-id="56160-213">Enfin, vous avez appris à modifier le comportement de la zone de liste déroulante en définissant ses propriétés DropDownStyle et AutoCompleteMode.</span><span class="sxs-lookup"><span data-stu-id="56160-213">Finally, you learned how to modify the behavior of the ComboBox by setting its DropDownStyle and AutoCompleteMode properties.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="56160-214">Précédent</span><span class="sxs-lookup"><span data-stu-id="56160-214">Previous</span></span>](how-do-i-use-the-combobox-control-cs.md)