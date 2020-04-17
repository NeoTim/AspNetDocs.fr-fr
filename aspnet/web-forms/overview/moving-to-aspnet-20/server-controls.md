---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Contrôles des serveurs (fr) Microsoft Docs
author: rick-anderson
description: ASP.NET 2.0 améliore les contrôles des serveurs de plusieurs façons. Dans ce module, nous couvrirons quelques-uns des changements architecturaux de la façon dont ASP.NET 2.0 et Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 7109f10e87abfadf1e7e08795cf9d3d6bf5df122
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543741"
---
# <a name="server-controls"></a>Contrôles serveur

par [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 améliore les contrôles des serveurs de plusieurs façons. Dans ce module, nous couvrirons quelques-uns des changements architecturaux de la façon dont ASP.NET 2.0 et Visual Studio 2005 traite des contrôles des serveurs.

ASP.NET 2.0 améliore les contrôles des serveurs de plusieurs façons. Dans ce module, nous couvrirons quelques-uns des changements architecturaux de la façon dont ASP.NET 2.0 et Visual Studio 2005 traite des contrôles des serveurs.

## <a name="view-state"></a>Afficher l’état

Le principal changement dans l’état de vue dans ASP.NET 2.0 est une réduction spectaculaire de la taille. Considérez une page avec seulement un contrôle de calendrier dessus. Voici l’état de vue dans ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Maintenant, voici l’état de vue sur une page identique dans ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

C’est un changement assez important, et compte tenu de cet état de vue est porté d’avant en arrière sur le fil, ce changement peut donner aux développeurs une augmentation significative des performances. La réduction de la taille de l’état de vue est en grande partie due à la façon dont nous le traitons à l’interne. Rappelez-vous que l’état de vue est une chaîne codée Base64. Pour mieux comprendre l’état de changement de vue dans ASP.NET 2.0, jetons un coup d’oeil aux valeurs décodées des exemples ci-dessus.

Voici l’état de vue 1.1 décodé :

[!code-css[Main](server-controls/samples/sample3.css)]

Cela peut ressembler un peu à du charabia, mais il ya un modèle ici. Dans ASP.NET 1.x, nous avons utilisé des caractères uniques pour identifier &lt; &gt; les types de données et les valeurs délimitées à l’aide des caractères. Le "t" dans l’échantillon d’état de vue ci-dessus représente un Triplet. Le Triplet contient une paire de ArrayLists (le "l" représente un ArrayList.) Un de ces ArrayLists contient un Int32 ("i") avec une valeur de 1 et l’autre contient un autre Triplet. Le Triplet contient une paire de ArrayLists, etc. La chose importante à retenir est que nous utilisons des triplets qui contiennent des &lt; paires, nous identifions les types de données via une lettre, et nous utilisons le et &gt; les caractères comme délimitants.

Dans ASP.NET 2.0, l’état de vue décodé semble un peu différent.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Vous devriez remarquer un énorme changement dans l’apparence de l’état de vue décodé. Ce changement a plusieurs fondements architecturaux. Afficher l’état dans ASP.NET 1.x utilisé le LosFormatter pour sérialiser les données. En 2.0, nous utilisons la nouvelle classe ObjectStateFormatter. Cette classe a été spécifiquement conçue pour aider à la sérialisation et à la désétérialisation de l’état de vue et de l’état de contrôle. (L’état de contrôle sera couvert dans la section suivante.) Il y a beaucoup d’avantages gagnés en changeant la méthode par laquelle la sérialisation et la désétérialisation ont lieu. L’un des plus dramatiques est le fait que contrairement au LosFormatter qui utilise un TextWriter, l’ObjectStateFormatter utilise un BinaryWriter. Cela permet à ASP.NET 2.0 de stocker l’état de vue une série d’octets au lieu de cordes. Prenez, par exemple, un intégrant. En ASP.NET 1.1, un intégreur a eu besoin de 4 octets de vue état. Dans ASP.NET 2.0, cette même integer ne nécessite que 1 byte. D’autres améliorations ont été apportées pour diminuer la quantité d’état de vue qui est stockée. Les valeurs DateTime, par exemple, sont maintenant stockées à l’aide d’un TickCount au lieu d’une chaîne.

Comme si tout cela ne suffisait pas, une attention particulière a été accordée au fait que l’un des plus grands consommateurs de l’état de vue en 1.x était le DataGrid et des contrôles similaires. Un inconvénient majeur des contrôles tels que le DataGrid en ce qui concerne l’état de vue est qu’il contient souvent de grandes quantités d’informations répétées. Dans ASP.NET 1.x, cette information répétée a simplement été stockée maintes et maintes fois, ce qui a donné lieu à un état de vue gonflé. Dans ASP.NET 2.0, nous utilisons la nouvelle classe IndexedString pour stocker de telles données. Si une chaîne se répète, nous stockons simplement le jeton pour l’IndexedString et l’index dans un tableau en cours d’exécution des objets IndexedString.

## <a name="control-state"></a>État de contrôle

L’un des principaux reproches que les développeurs avaient avec l’état de vue était la taille qu’il a ajouté à la charge utile HTTP. Comme mentionné précédemment, l’un des plus grands consommateurs de l’état de vue est le contrôle DataGrid. Pour éviter les énormes quantités d’état de vue générée par un DataGrid, de nombreux développeurs simplement désactivé afficher l’état pour ce contrôle. Malheureusement, cette solution n’a pas toujours été bonne. Afficher l’état dans ASP.NET 1.x contient non seulement les données nécessaires pour la fonctionnalité correcte du contrôle. Il contient également des informations concernant l’état de l’interface utilisateur du contrôle. Cela signifie que si vous voulez permettre la pagination sur un DataGrid, vous devez activer l’état de vue même si vous n’avez pas besoin de toutes les informations d’interface utilisateur que l’état de vue contient. C’est un scénario tout ou rien.

Dans ASP.NET 2.0, l’état de contrôle résout bien ce problème par l’introduction de l’état de contrôle. L’état de contrôle contient les données qui sont absolument nécessaires pour la fonctionnalité appropriée d’un contrôle. Contrairement à l’état de vue, l’état de contrôle ne peut pas être désactivé. Par conséquent, il est important que les données stockées dans l’état de contrôle soient soigneusement contrôlées.

> [!NOTE]
> L’état de contrôle persiste avec \_ \_l’état de vue dans le champ de forme cachée VIEWSTATE.

Cette vidéo est un état de vision et de contrôle pas à pas.

![](server-controls/_static/image1.png)

[Ouvrez la vidéo plein écran](server-controls/_static/state1.wmv)

Pour qu’un contrôle de serveur soit lu et écrit pour contrôler l’état, vous devez prendre trois étapes.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Étape 1 : Appelez la méthode RegisterRequiresControlState

La méthode RegisterRequiresControlState informe ASP.NET qu’un contrôle doit persister à contrôler l’état. Il faut un argument de type Contrôle qui est le contrôle qui est enregistré.

Il est important de noter que l’enregistrement ne persiste pas de la demande. Par conséquent, cette méthode doit être appelée sur chaque demande si un contrôle est de persister état de contrôle. Il est recommandé que la méthode soit appelée dans OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Étape 2 : Remplacer SaveControlState

La méthode SaveControlState permet d’économiser des changements d’état de contrôle pour un contrôle depuis le dernier poteau de retour. Il renvoie un objet représentant l’état du contrôle.

## <a name="step-3-override-loadcontrolstate"></a>Étape 3 : Override LoadControlState

Les charges de méthode LoadControlState ont sauvé l’état dans un contrôle. La méthode prend un argument de type Objet qui détient l’état sauvé pour le contrôle.

## <a name="full-xhtml-compliance"></a>Conformité XHTML complète

Tout développeur Web connaît l’importance des normes dans les applications Web. Afin de maintenir un environnement de développement basé sur des normes, ASP.NET 2.0 est entièrement conforme À la XHTML. Par conséquent, toutes les balises sont rendues selon les normes XHTML dans les navigateurs qui prennent en charge HTML 4.0 ou plus.

La définition DOCTYPE dans ASP.NET 1.1 était la suivante :

[!code-html[Main](server-controls/samples/sample6.html)]

Dans ASP.NET 2.0, la définition DOCTYPE par défaut est la suivante :

[!code-html[Main](server-controls/samples/sample7.html)]

Si vous le souhaitez, vous pouvez modifier la conformité XHTML par défaut via le nœud xhtmlConformance dans le fichier de configuration. Par exemple, le nœud suivant dans le fichier web.config modifiera la conformité XHTML à XHTML 1.0 Strict :

[!code-xml[Main](server-controls/samples/sample8.xml)]

Si vous le souhaitez, vous pouvez également configurer ASP.NET pour utiliser la configuration héritée utilisée dans ASP.NET 1.x comme suit :

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Rendu adaptatif à l’aide d’adaptateurs

Dans ASP.NET 1.x, le fichier de &lt;configuration&gt; contenait une section browserCaps qui a peuplé un objet HttpBrowserCapabilities. Cet objet a permis à un développeur de déterminer quel appareil fait une demande particulière et de rendre le code de manière appropriée. Dans ASP.NET 2.0, le modèle s’est amélioré et utilise maintenant la nouvelle classe ControlAdapter. La classe ControlAdapter remplace les événements du cycle de vie du contrôleur et contrôle le rendu des contrôles en fonction des capacités de l’agent utilisateur. Les capacités d’un agent utilisateur spécifique sont définies par un fichier de définition de navigateur (un fichier avec une extension de fichier .browser) stocké dans le c: 'windows’microsoft.net-framework’v2.0. \* \*Dossier \* \*

> [!NOTE]
> La classe ControlAdapter est une classe abstraite.

Tout comme &lt;la&gt; section browserCaps en 1.x, le fichier de définition du navigateur utilise une expression régulière pour analyser la chaîne de l’agent utilisateur afin d’identifier le navigateur demandeur. Il définit les capacités particulières pour cet agent utilisateur. Le ControlAdapter rend le contrôle via la méthode Render. Par conséquent, si vous remplacez la méthode Render, vous ne devez pas appeler Render sur la classe de base. Cela peut provoquer un rendu deux fois, une fois pour l’adaptateur et une fois pour le contrôle lui-même.

## <a name="developing-a-custom-adapter"></a>Développer un adaptateur personnalisé

Vous pouvez développer votre propre adaptateur personnalisé en héritant de ControlAdapter. En outre, vous pouvez hériter de la classe abstraite PageAdapter dans les cas où un adaptateur est nécessaire pour une page. La cartographie des contrôles de votre &lt;adaptateur&gt; personnalisé est effectuée via l’élément controlAdapters dans le fichier de définition du navigateur. Par exemple, le XML suivant à partir d’un fichier de définition de navigateur cartographie le contrôle du menu à la classe MenuAdapter :

[!code-html[Main](server-controls/samples/sample10.html)]

En utilisant ce modèle, il devient assez facile pour un développeur de contrôle de cibler un appareil ou un navigateur particulier. Il est également assez simple pour un développeur d’avoir un contrôle complet sur la façon dont les pages rendent sur chaque appareil.

## <a name="per-device-rendering"></a>Rendu par appareil

Les propriétés de contrôle du serveur dans ASP.NET 2.0 peuvent être spécifiées par appareil à l’aide d’un préfixe spécifique au navigateur. Par exemple, le code ci-dessous modifiera le texte d’une étiquette en fonction de l’appareil utilisé pour parcourir la page.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Lorsque la page contenant cette étiquette est parcourue à partir d’Internet Explorer, l’étiquette affiche le texte disant « Vous naviguez à partir d’Internet Explorer ». Lorsque la page est parcourue à partir de Firefox, l’étiquette affiche le texte "Vous naviguez à partir de Firefox." Lorsque la page est parcourue à partir de n’importe quel autre appareil, elle affiche « Vous naviguez à partir d’un appareil inconnu ». Toute propriété peut être spécifiée à l’aide de cette syntaxe spéciale.

## <a name="setting-focus"></a>Mise au point

ASP.NET développeurs de 1,0 ont souvent demandé comment mettre l’accent initial sur un contrôle particulier. Par exemple, sur une page de connexion, il est utile que la boîte de texte d’identification utilisateur se concentre lorsque la page se charge. Dans ASP.NET 1.x, faire cela a nécessité l’écriture de certains script côté client. Même si un tel script est une tâche triviale, il n’est plus nécessaire dans ASP.NET 2.0 grâce à la méthode SetFocus. La méthode SetFocus prend un argument indiquant le contrôle qui devrait recevoir la mise au point. Cet argument peut être soit l’ID client du contrôle comme une chaîne ou le nom du contrôle serveur comme objet de contrôle. Par exemple, pour définir l’accent initial sur un contrôle TextBox appelé txtUserID\_lorsque la page se charge, ajoutez le code suivant à Page Load :

[!code-csharp[Main](server-controls/samples/sample12.cs)]

-- ou

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 utilise le gestionnaire Webresource.axd (discuté précédemment) pour rendre une fonction côté client qui met l’accent. Le nom de la fonction client-côté est WebForm\_AutoFocus comme indiqué ici:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternativement, vous pouvez utiliser la méthode Focus pour un contrôle pour définir l’accent initial à ce contrôle. La méthode Focus provient de la classe De contrôle et est disponible pour tous les contrôles ASP.NET 2.0. Il est également possible de mettre l’accent sur un contrôle particulier lorsqu’une erreur de validation se produit. Cela sera couvert dans un module ultérieur.

## <a name="new-server-controls-in-aspnet-20"></a>Nouveaux contrôles de serveur en ASP.NET 2.0

Ce qui suit sont de nouveaux contrôles de serveur dans ASP.NET 2.0. Nous allons entrer dans plus de détails sur certains d’entre eux dans les modules ultérieurs.

## <a name="imagemap-control"></a>Contrôle ImageMap

Le contrôle ImageMap vous permet d’ajouter des points d’accès à une image qui peut initier un poste en arrière ou naviguer vers une URL. Il existe trois types de points chauds disponibles; CircleHotSpot, RectangleHotSpot et PolygonHotSpot. Les points chauds sont ajoutés via un éditeur de collection dans Visual Studio ou programmatiquement en code. Il n’existe pas d’interface utilisateur disponible pour dessiner des points d’accès sur une image. Les coordonnées et la taille ou le rayon du point chaud doivent être spécifiés de façon déclarative. Il n’y a pas non plus de représentation visuelle d’un hotspot dans le concepteur. Si un point d’accès est configuré pour naviguer vers une URL, l’URL est spécifiée via la propriété NavigateUrl du point d’accès. Dans le cas d’un point d’accès post back, la propriété PostBackValue vous permet de passer une chaîne dans le poste de retour qui peut être récupéré dans le code côté serveur.

![Rédacteur en chef de la collection HotSpot en studio visuel](server-controls/_static/image1.jpg)

**Figure 1**: Rédacteur en chef de la collection HotSpot dans Visual Studio

## <a name="bulletedlist-control"></a>Contrôle BulletedList

Le contrôle BulletedList est une liste de balles qui peut facilement être liée aux données. La liste peut être commandée (numérotée) ou non commandée via la propriété BulletStyle. Chaque élément de la liste est représenté par un objet ListItem.

![Contrôle BulletedList dans Visual Studio](server-controls/_static/image1.gif)

**Figure 2**: Contrôle BulletedList dans Visual Studio

## <a name="hiddenfield-control"></a>Contrôle HiddenField

Le contrôle HiddenField ajoute un champ de forme caché à votre page, dont la valeur est disponible dans le code côté serveur. On s’attend généralement à ce que la valeur d’un champ de forme caché reste inchangée entre les arrières postérieurs. Cependant, il est possible pour un utilisateur malveillant de changer la valeur avant de publier en arrière. Si cela se produit, le contrôle HiddenField augmentera l’événement ValueChanged. Si vous avez des informations sensibles dans le contrôle HiddenField et que vous souhaitez vous assurer qu’elles restent inchangées, vous devez gérer l’événement ValueChanged dans votre code.

## <a name="fileupload-control"></a>Contrôle de chargement de fichiers

Le contrôle FileUpload dans ASP.NET 2.0 permet de télécharger des fichiers sur un serveur Web via une page ASP.NET. Ce contrôle est assez similaire à la classe ASP.NET 1.x HtmlInputFile à quelques exceptions près. Dans ASP.NET 1.x, il a été recommandé que la propriété PostedFile soit vérifiée pour nullité afin de déterminer si vous aviez un bon fichier. Le contrôle FileUpload dans ASP.NET 2.0 ajoute une nouvelle propriété HasFile que vous pouvez utiliser dans le même but et il est un peu plus efficace.

La propriété PostedFile est toujours disponible pour accéder à un objet HttpPostedFile, mais certaines fonctionnalités de la HttpPostedFile est maintenant disponible intrinsèquement avec le contrôle FileUpload. Par exemple, pour enregistrer un fichier téléchargé dans ASP.NET 1.x, vous appelez la méthode SaveAs sur l’objet HttpPostedFile. En utilisant le contrôle FileUpload dans ASP.NET 2.0, vous appelez la méthode SaveAs sur le contrôle FileUpload lui-même.

Un autre changement significatif dans le comportement 2.0 (et probablement le changement le plus significatif) est qu’il n’est plus nécessaire de charger un fichier téléchargé entier dans la mémoire avant de l’enregistrer. Dans 1.x, tout fichier qui a été téléchargé est enregistré entièrement dans la mémoire avant d’être écrit sur disque. Cette architecture empêche le téléchargement de fichiers volumineux.

Dans ASP.NET 2.0, l’attribut requestLengthDiskThreshold de l’élément httpRuntime vous permet de configurer le nombre de kilooctets sont conservés dans un tampon dans la mémoire avant d’être écrits sur disque.

**IMPORTANT**: La documentation MSDN (et la documentation ailleurs) précise que cette valeur est dans les octets (et non les kilooctets) et que la valeur par défaut est de 256. La valeur est en fait spécifiée dans Kilooctets et la valeur par défaut est de 80. En ayant une valeur par défaut de 80K, nous nous assurons que le tampon ne se retrouve pas sur le tas de gros objets.

## <a name="wizard-control"></a>Contrôle des sorciers

Il est assez fréquent de rencontrer ASP.NET développeurs qui ont du mal à recueillir des informations dans une série de «pages» à l’aide de panneaux ou en transférant d’une page à l’autre. Le plus souvent, l’effort est frustrant et prend beaucoup de temps. Le nouveau contrôle Wizard résout les problèmes en permettant des étapes linéaires et non linéaires dans une interface assistant que les utilisateurs connaissent bien. Le contrôle Wizard présente des formulaires d’entrée dans une série d’étapes. Chaque étape est d’un type particulier spécifié par la propriété StepType du contrôle. Les types d’étapes disponibles sont les suivants :

| **Type d’étape** | **Explication** |
| --- | --- |
| Auto | L’assistant détermine automatiquement le type d’étape en fonction de sa position dans la hiérarchie de l’étape. |
| Démarrer | La première étape, souvent utilisée pour présenter une déclaration d’introduction. |
| Étape | Une étape normale. |
| Finish | La dernière étape, habituellement utilisé pour présenter un bouton pour terminer l’assistant. |
| Terminé | Présente un message communiquant le succès ou l’échec. |

> [!NOTE]
> Le contrôle Wizard garde une trace de son état en utilisant ASP.NET’état de contrôle. Par conséquent, la propriété EnableViewState peut être mise à faux sans aucun préjudice.

Cette vidéo est une procédure pas à pas du contrôle Wizard.

![](server-controls/_static/image2.png)

[Ouvrez la vidéo plein écran](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Localiser le contrôle

Le contrôle de localisation est similaire à un contrôle littéral. Cependant, le contrôle Localize a une propriété **Mode** qui contrôle la façon dont la majoration qui est ajoutée à elle est rendue. La propriété Mode prend en charge les valeurs suivantes :

| **Mode** | **Explication** |
| --- | --- |
| Transformer | Le balisage est transformé selon le protocole du navigateur faisant la demande. |
| Passthrough | Markup est rendu tel qu’il est. |
| Encoder | Le balisage qui est ajouté au contrôle est codé à l’aide de HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Contrôles MultiView et Vue

Le contrôle MultiView agit comme un conteneur pour les commandes View, et le contrôle View agit comme un conteneur (un peu comme un contrôle du panneau) pour d’autres contrôles. Chaque vue dans un contrôle MultiView est représentée par un seul contrôle De vue. Le premier contrôle De vue dans le MultiView est vue 0, le second est vue 1, etc. Vous pouvez changer de vue en spécifiant l’ActiveViewIndex du contrôle MultiView.

## <a name="substitution-control"></a>Contrôle de substitution

Le contrôle de substitution est utilisé en conjonction avec ASP.NET mise en cache. Dans les cas où vous souhaitez profiter de la mise en cache, mais vous avez des parties d’une page qui doivent être mises à jour sur chaque demande (en d’autres termes, des parties d’une page qui sont exemptées de mise en cache), le composant De substitution fournit une excellente solution. Le contrôle ne rend pas réellement toute sortie sur son propre. Au lieu de cela, il est lié à une méthode dans le code côté serveur. Lorsque la page est demandée, la méthode est appelée et la majoration retournée est rendue à la place du contrôle de substitution.

La méthode à laquelle le contrôle de substitution est lié est spécifiée via la propriété **MethodName.** Cette méthode doit répondre aux critères suivants :

- Il doit s’agir d’une méthode statique (partagée en VB).
- Il accepte un paramètre de type HttpContext.
- Il renvoie une chaîne représentant la majoration qui devrait remplacer le contrôle sur la page.

Le contrôle de substitution n’a pas la possibilité de modifier tout autre contrôle sur la page, mais il a accès au texte HttpCon en cours via son paramètre.

## <a name="gridview-control"></a>Contrôle GridView

Le contrôle GridView est le remplacement du contrôle DataGrid. Ce contrôle sera couvert plus en détail dans un module ultérieur.

## <a name="detailsview-control"></a>DétailsView Control (en)

Le contrôle DetailsView vous permet d’afficher un seul enregistrement à partir d’une source de données et de les modifier ou de les supprimer. Il est couvert plus en détail dans un module ultérieur.

## <a name="formview-control"></a>Contrôle FormView

Le contrôle FormView est utilisé pour afficher un seul enregistrement à partir d’une source de données dans une interface configurable. Il est couvert plus en détail dans un module ultérieur.

## <a name="accessdatasource-control"></a>Contrôle AccessDataSource

Le contrôle AccessDataSource est utilisé pour lier les données d’une base de données d’accès. Il est couvert plus en détail dans un module ultérieur.

## <a name="objectdatasource-control"></a>Contrôle ObjectDataSource

Le contrôle ObjectDataSource est utilisé pour prendre en charge une architecture à trois niveaux afin que les contrôles puissent être liés à des données à un objet d’affaires de niveau intermédiaire par opposition à un modèle à deux niveaux où les contrôles sont liés directement à la source de données. Il sera discuté plus en détail dans un module ultérieur.

## <a name="xmldatasource-control"></a>Contrôle XmlDataSource

Le contrôle XmlDataSource est utilisé pour lier les données à une source de données XML. Il est couvert plus en détail dans un module ultérieur.

## <a name="sitemapdatasource-control"></a>Contrôle SiteMapDataSource

Le contrôle SiteMapDataSource fournit une liaison de données pour les contrôles de navigation sur site basé sur une carte du site. Il sera discuté plus en détail dans un module ultérieur.

## <a name="sitemappath-control"></a>Contrôle SiteMapPath

Le contrôle SiteMapPath affiche une série de liaisons de navigation communément appelées chapelure. Il est couvert plus en détail dans un module ultérieur.

## <a name="menu-control"></a>Contrôle Menu

Le contrôle du menu affiche des menus dynamiques à l’aide de DHTML. Il est couvert plus en détail dans un module ultérieur.

## <a name="treeview-control"></a>TreeView, contrôle

Le contrôle TreeView est utilisé pour afficher une vue hiérarchique des données sur les arbres. Il est couvert plus en détail dans un module ultérieur.

## <a name="login-control"></a>Contrôle de la connexion

Le contrôle de connexion prévoit un mécanisme pour se connecter à un site Web. Il est couvert plus en détail dans un module ultérieur.

## <a name="loginview-control"></a>Contrôle LoginView

Le contrôle LoginView permet l’affichage de différents modèles en fonction de l’état de connexion d’un utilisateur. Il est couvert plus en détail dans un module ultérieur.

## <a name="passwordrecovery-control"></a>Contrôle de PasswordRecovery

Le contrôle PasswordRecovery est utilisé pour récupérer les mots de passe oubliés par les utilisateurs d’une application ASP.NET. Il est couvert plus en détail dans un module ultérieur.

## <a name="loginstatus"></a>LoginStatus

Le contrôle LoginStatus affiche l’état de connexion d’un utilisateur. Il est couvert plus en détail dans un module ultérieur.

## <a name="loginname"></a>LoginName

Le contrôle LoginName affiche le nom d’utilisateur d’un utilisateur après avoir été connecté à une application ASP.NET. Il est couvert plus en détail dans un module ultérieur.

## <a name="createuserwizard"></a>CréerUserWizard

Le CreateUserWizard est un assistant configurable qui donne aux utilisateurs la possibilité de créer un compte d’adhésion ASP.NET pour une utilisation dans une application ASP.NET. Il est couvert plus en détail dans un module ultérieur.

## <a name="changepassword"></a>ChangePassword

Le contrôle ChangePassword permet aux utilisateurs de changer leur mot de passe pour une application ASP.NET. Il est couvert plus en détail dans un module ultérieur.

## <a name="various-webparts"></a>Divers WebParts

ASP.NET 2.0 navires avec diverses pièces Web. Ceux-ci seront couverts en détail dans un module ultérieur.
