---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Envoi de données de formulaire HTML dans l’API Web ASP.NET : Chargement de fichier et MIME à parties multiples | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 875f9ac62901dfbafc8224af2982c1daf3afc9c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028976"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="b526a-102">Envoi de données de formulaire HTML dans l’API Web ASP.NET : chargement de fichier et MIME Multipart</span><span class="sxs-lookup"><span data-stu-id="b526a-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="b526a-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b526a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="b526a-104">Partie 2 : chargement de fichier et MIME Multipart</span><span class="sxs-lookup"><span data-stu-id="b526a-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="b526a-105">Ce didacticiel montre comment charger des fichiers à une API web.</span><span class="sxs-lookup"><span data-stu-id="b526a-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="b526a-106">Il décrit également comment traiter des données MIME en plusieurs parties.</span><span class="sxs-lookup"><span data-stu-id="b526a-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="b526a-107">[Télécharger le projet achevé](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="b526a-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="b526a-108">Voici un exemple d’un formulaire HTML pour le chargement d’un fichier :</span><span class="sxs-lookup"><span data-stu-id="b526a-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="b526a-109">Ce formulaire contienne un contrôle d’entrée de texte et un contrôle d’entrée de fichier.</span><span class="sxs-lookup"><span data-stu-id="b526a-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="b526a-110">Lorsqu’un formulaire contient un contrôle d’entrée de fichier, le **enctype** attribut doit toujours être &quot;multipart/form-data&quot;, qui spécifie que le formulaire sera envoyé comme un message MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="b526a-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="b526a-111">Le format d’un message MIME à parties multiples est plus facile d’appréhender en examinant un exemple de demande :</span><span class="sxs-lookup"><span data-stu-id="b526a-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="b526a-112">Ce message est divisé en deux *parties*, un pour chaque contrôle de formulaire.</span><span class="sxs-lookup"><span data-stu-id="b526a-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="b526a-113">Limites de la partie sont indiquées par les lignes qui commencent par des tirets.</span><span class="sxs-lookup"><span data-stu-id="b526a-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="b526a-114">La limite de la partie inclut un composant aléatoire (&quot;41184676334&quot;) pour vous assurer que la chaîne de limite n’apparaît pas par inadvertance à l’intérieur d’une partie de message.</span><span class="sxs-lookup"><span data-stu-id="b526a-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="b526a-115">Chaque partie de message contient un ou plusieurs en-têtes, suivies du contenu de la partie.</span><span class="sxs-lookup"><span data-stu-id="b526a-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="b526a-116">L’en-tête Content-Disposition inclut le nom du contrôle.</span><span class="sxs-lookup"><span data-stu-id="b526a-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="b526a-117">Pour les fichiers, il contient également le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="b526a-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="b526a-118">L’en-tête Content-Type décrit les données dans la partie.</span><span class="sxs-lookup"><span data-stu-id="b526a-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="b526a-119">Si cet en-tête est omis, la valeur par défaut est text/plain.</span><span class="sxs-lookup"><span data-stu-id="b526a-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="b526a-120">Dans l’exemple précédent, l’utilisateur a chargé un fichier nommé GrandCanyon.jpg, avec le type de contenu image/jpeg ; et la valeur de l’entrée de texte était &quot;vacances d’été&quot;.</span><span class="sxs-lookup"><span data-stu-id="b526a-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="b526a-121">Chargement du fichier</span><span class="sxs-lookup"><span data-stu-id="b526a-121">File Upload</span></span>

<span data-ttu-id="b526a-122">Maintenant nous allons examiner un contrôleur d’API Web qui lit des fichiers à partir d’un message MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="b526a-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="b526a-123">Le contrôleur lit les fichiers de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="b526a-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="b526a-124">API Web prend en charge les actions asynchrones à l’aide de la [modèle de programmation basé sur des tâches](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="b526a-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="b526a-125">Tout d’abord, voici le code si vous ciblez .NET Framework 4.5, qui prend en charge la **async** et **await** mots clés.</span><span class="sxs-lookup"><span data-stu-id="b526a-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="b526a-126">Notez que l’action du contrôleur ne prend pas de paramètres.</span><span class="sxs-lookup"><span data-stu-id="b526a-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="b526a-127">C’est parce que nous traiter le corps de la demande à l’intérieur de l’action, sans appeler un formateur de type de média.</span><span class="sxs-lookup"><span data-stu-id="b526a-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="b526a-128">Le **IsMultipartContent** méthode vérifie si la demande contient un message MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="b526a-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="b526a-129">Si ce n’est pas le cas, le contrôleur retourne le code d’état HTTP 415 (Type de média non pris en charge).</span><span class="sxs-lookup"><span data-stu-id="b526a-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="b526a-130">Le **MultipartFormDataStreamProvider** classe est un objet d’assistance qui alloue des flux de fichiers pour les fichiers chargés.</span><span class="sxs-lookup"><span data-stu-id="b526a-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="b526a-131">Pour lire le message MIME à parties multiples, appelez le **ReadAsMultipartAsync** (méthode).</span><span class="sxs-lookup"><span data-stu-id="b526a-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="b526a-132">Cette méthode extrait toutes les parties de message et les écrit dans les flux fournis par le **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="b526a-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="b526a-133">Lorsque la méthode est terminée, vous pouvez obtenir des informations sur les fichiers à partir de la **FileData** propriété, qui est une collection de **MultipartFileData** objets.</span><span class="sxs-lookup"><span data-stu-id="b526a-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="b526a-134">**MultipartFileData.FileName** est le nom de fichier local sur le serveur, où le fichier a été enregistré.</span><span class="sxs-lookup"><span data-stu-id="b526a-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="b526a-135">**MultipartFileData.Headers** contient l’en-tête de la partie (*pas* l’en-tête de demande).</span><span class="sxs-lookup"><span data-stu-id="b526a-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="b526a-136">Vous pouvez l’utiliser pour accéder au contenu\_les en-têtes Content-Type et la destruction.</span><span class="sxs-lookup"><span data-stu-id="b526a-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="b526a-137">Comme son nom l’indique, **ReadAsMultipartAsync** est une méthode asynchrone.</span><span class="sxs-lookup"><span data-stu-id="b526a-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="b526a-138">Pour effectuer le travail une fois que la méthode est terminée, utilisez un [tâche de continuation](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) ou le **await** mot clé (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="b526a-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="b526a-139">Voici la version de .NET Framework 4.0 du code précédent :</span><span class="sxs-lookup"><span data-stu-id="b526a-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="b526a-140">Lire les données de contrôle de formulaire</span><span class="sxs-lookup"><span data-stu-id="b526a-140">Reading Form Control Data</span></span>

<span data-ttu-id="b526a-141">Le formulaire HTML que je vous ai montré précédemment avait un contrôle d’entrée de texte.</span><span class="sxs-lookup"><span data-stu-id="b526a-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="b526a-142">Vous pouvez obtenir la valeur du contrôle à partir de la **FormData** propriété de la **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="b526a-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="b526a-143">**FormData** est un **NameValueCollection** qui contient les paires nom/valeur pour les contrôles du formulaire.</span><span class="sxs-lookup"><span data-stu-id="b526a-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="b526a-144">La collection peut contenir des clés en double.</span><span class="sxs-lookup"><span data-stu-id="b526a-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="b526a-145">Considérez ce formulaire :</span><span class="sxs-lookup"><span data-stu-id="b526a-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="b526a-146">Le corps de la requête peut se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="b526a-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="b526a-147">Dans ce cas, le **FormData** collection contient les paires clé/valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="b526a-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="b526a-148">voyage : aller-retour</span><span class="sxs-lookup"><span data-stu-id="b526a-148">trip: round-trip</span></span>
- <span data-ttu-id="b526a-149">options : sans interruption</span><span class="sxs-lookup"><span data-stu-id="b526a-149">options: nonstop</span></span>
- <span data-ttu-id="b526a-150">options : dates</span><span class="sxs-lookup"><span data-stu-id="b526a-150">options: dates</span></span>
- <span data-ttu-id="b526a-151">siège : fenêtre</span><span class="sxs-lookup"><span data-stu-id="b526a-151">seat: window</span></span>
