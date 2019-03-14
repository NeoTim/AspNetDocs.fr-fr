---
ms.openlocfilehash: 939231583679d469d01724cc1a405bd45e46d2c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57175886"
---
```console
npm run release
```

<span data-ttu-id="95f8f-101">Cette commande génère les ressources côté client à délivrer pendant l’exécution de l’application.</span><span class="sxs-lookup"><span data-stu-id="95f8f-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="95f8f-102">Les ressources sont placées dans le dossier *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="95f8f-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="95f8f-103">Webpack a effectué les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="95f8f-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="95f8f-104">Purge du contenu du répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="95f8f-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="95f8f-105">Conversion du code TypeScript en JavaScript&mdash;processus appelé *transpilation*.</span><span class="sxs-lookup"><span data-stu-id="95f8f-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="95f8f-106">Troncation du code JavaScript généré pour réduire la taille de fichier&mdash;processus appelé *minimisation*.</span><span class="sxs-lookup"><span data-stu-id="95f8f-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="95f8f-107">Copie des fichiers JavaScript, CSS et HTML traités à partir de *src* dans le répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="95f8f-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="95f8f-108">Injection des éléments suivants dans le fichier *wwwroot/index.html* :</span><span class="sxs-lookup"><span data-stu-id="95f8f-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
    * <span data-ttu-id="95f8f-109">Une balise `<link>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css*.</span><span class="sxs-lookup"><span data-stu-id="95f8f-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="95f8f-110">Cette balise est placée immédiatement avant la balise `</head>` de fermeture.</span><span class="sxs-lookup"><span data-stu-id="95f8f-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
    * <span data-ttu-id="95f8f-111">Une balise `<script>`, référençant le fichier *wwwroot/main.\<code de hachage\>.css* minimisé.</span><span class="sxs-lookup"><span data-stu-id="95f8f-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="95f8f-112">Cette balise est placée immédiatement avant la balise `</body>` de fermeture.</span><span class="sxs-lookup"><span data-stu-id="95f8f-112">This tag is placed immediately before the closing `</body>` tag.</span></span>
