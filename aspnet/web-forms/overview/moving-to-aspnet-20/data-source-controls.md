---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Contrôles des sources de données (fr) Microsoft Docs
author: rick-anderson
description: Le contrôle DataGrid dans ASP.NET 1.x a marqué une grande amélioration de l’accès aux données dans les applications Web. Cependant, il n’était pas aussi convivial qu’il aurait pu l’être ....
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: 2b4302b509af57dc5d9db9de9ee824df767d0737
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540218"
---
# <a name="data-source-controls"></a>Contrôles de source de données

par [Microsoft](https://github.com/microsoft)

> Le contrôle DataGrid dans ASP.NET 1.x a marqué une grande amélioration de l’accès aux données dans les applications Web. Cependant, il n’était pas aussi convivial qu’il aurait pu l’être. Il fallait encore une quantité considérable de code pour obtenir beaucoup de fonctionnalités utiles de lui. Tel est le modèle dans toutes les entreprises d’accès aux données en 1.x.

Le contrôle DataGrid dans ASP.NET 1.x a marqué une grande amélioration de l’accès aux données dans les applications Web. Cependant, il n’était pas aussi convivial qu’il aurait pu l’être. Il fallait encore une quantité considérable de code pour obtenir beaucoup de fonctionnalités utiles de lui. Tel est le modèle dans toutes les entreprises d’accès aux données en 1.x.

ASP.NET 2.0 aborde cela en partie avec des contrôles de source de données. Les contrôles de source de données dans ASP.NET 2.0 fournissent aux développeurs un modèle déclaratif pour récupérer des données, afficher des données et modifier des données. Le but des contrôles des sources de données est de fournir une représentation cohérente des données aux contrôles liés aux données, quelle que soit la source de ces données. Au cœur des contrôles de source de données dans ASP.NET 2.0 se trouve la classe abstraite DataSourceControl. La classe DataSourceControl fournit une mise en œuvre de base de l’interface IDataSource et de l’interface IListSource, ce qui vous permet d’attribuer le contrôle de source de données comme DataSource d’un contrôle lié aux données (via la nouvelle propriété DataSourceId discutée plus tard) et d’exposer les données dont vous disposez comme une liste. Chaque liste de données provenant d’un contrôle de source de données est exposée comme un objet DataSourceView. L’accès aux instances DataSourceView est assuré par l’interface IDataSource. Par exemple, la méthode GetViewNames renvoie une ICollection qui vous permet d’énumérer les DataSourceViews associés à un contrôle de source de données particulier, et la méthode GetView vous permet d’accéder à une instance DataSourceView particulière par son nom.

Les contrôles de source de données n’ont pas d’interface utilisateur. Ils sont implémentés comme contrôles de serveur afin qu’ils puissent prendre en charge la syntaxe déclarative et afin qu’ils aient accès à l’état de page si désiré. Les contrôles de source de données ne rendent aucune marque HTML au client.

> [!NOTE]
> Comme vous le verrez plus tard, il y a également des avantages de mise en cache obtenus à l’aide de contrôles source de données.

## <a name="storing-connection-strings"></a>Stockage des chaînes de connexion

Avant d’examiner comment configurer les contrôles de source de données, nous devrions couvrir une nouvelle capacité dans ASP.NET 2.0 concernant les chaînes de connexion. ASP.NET 2.0 introduit une nouvelle section dans le fichier de configuration qui vous permet de stocker facilement des chaînes de connexion qui peuvent être lues dynamiquement au moment de l’exécution. La &lt;section connectionStrings&gt; facilite le stockage des chaînes de connexion.

L’extrait ci-dessous ajoute une nouvelle chaîne de connexion.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Tout comme &lt;avec la section&gt; appSettings, la &lt;section connectionStrings&gt; apparaît en dehors de la &lt;section system.web&gt; dans le fichier de configuration.

Pour utiliser cette chaîne de connexion, vous pouvez utiliser la syntaxe suivante lors de la définition de l’attribut ConnectionString d’un contrôle serveur.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

La &lt;section connexionStrings peut également être cryptée de sorte que les informations sensibles&gt; ne soient pas exposées. Cette capacité sera couverte dans un module ultérieur.

## <a name="caching-data-sources"></a>Sources de données de cachage

Chaque DataSourceControl fournit quatre propriétés pour configurer la mise en cache; EnableCaching, CacheDuration, CacheExpirationPolicy et CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching (en)

EnableCaching est une propriété Boolean qui détermine si la mise en cache est activée ou non pour le contrôle de la source de données.

## <a name="cacheduration-property"></a>Propriété CacheDuration

La propriété CacheDuration définit le nombre de secondes que le cache reste valide. La mise en place de cette propriété à **0** fait que le cache reste valide jusqu’à ce qu’il soit explicitement invalidé.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy Propriété

La propriété CacheExpirationPolicy peut être réglée soit à **Absolute** ou **Sliding**. Le définir à Absolute signifie que le temps maximum que les données seront mis en cache est le nombre de secondes spécifiées par la propriété CacheDuration. En le définissant en glissement, le temps d’expiration est réinitialisé lorsque chaque opération est effectuée.

## <a name="cachekeydependency-property"></a>Propriété CacheKeyDependency

Si une valeur de chaîne est spécifiée pour la propriété CacheKeyDependency, ASP.NET mettra en place une nouvelle dépendance de cache basée sur cette chaîne. Cela vous permet d’invalider explicitement le cache en changeant ou en supprimant simplement la compétence CacheKey.

**Important**: Si l’usurpation d’identité est activée et que l’accès à la source de données et/ou au contenu des données est basé sur l’identité du client, il est recommandé que la mise en cache soit désactivée en définissant EnableCaching to False. Si la mise en cache est activée dans ce scénario et qu’un utilisateur autre que l’utilisateur qui a demandé à l’origine les données émet une demande, l’autorisation à la source de données n’est pas appliquée. Les données seront simplement servies à partir du cache.

## <a name="the-sqldatasource-control"></a>Le contrôle SqlDataSource

Le contrôle SqlDataSource permet à un développeur d’accéder aux données stockées dans n’importe quelle base de données relationnelle qui prend en charge ADO.NET. Il peut utiliser le fournisseur System.Data.SqlClient pour accéder à une base de données SQL Server, le fournisseur System.Data.OleDb, le fournisseur System.Data.Odbc, ou le fournisseur System.Data.OracleClient pour accéder à Oracle. Par conséquent, le SqlDataSource n’est certainement pas seulement utilisé pour accéder aux données dans une base de données SQL Server.

Afin d’utiliser le SqlDataSource, vous fournissez simplement une valeur pour la propriété ConnectionString et spécifiez une commande SQL ou une procédure stockée. Le contrôle SqlDataSource s’occupe de travailler avec l’architecture sous-jacente ADO.NET. Il ouvre la connexion, interroge la source de données ou exécute la procédure stockée, renvoie les données, puis ferme la connexion pour vous.

> [!NOTE]
> Étant donné que la classe DataSourceControl ferme automatiquement la connexion pour vous, elle doit réduire le nombre d’appels des clients générés par la fuite de connexions de base de données.

L’extrait de code ci-dessous lie un contrôle DropDownList à un contrôle SqlDataSource à l’aide de la chaîne de connexion qui est stockée dans le fichier de configuration comme indiqué ci-dessus.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Comme illustré ci-dessus, la propriété DataSourceMode du SqlDataSource spécifie le mode pour la source de données. Dans l’exemple ci-dessus, le DataSourceMode est réglé sur DataReader. Dans ce cas, le SqlDataSource retournera un objet IDataReader à l’aide d’un curseur avant et lu uniquement. Le type spécifié d’objet retourné est contrôlé par le fournisseur qui est utilisé. Dans ce cas, j’utilise le fournisseur System.Data.SqlClient tel que spécifié dans la &lt;section connectionStrings&gt; du fichier web.config. Par conséquent, l’objet qui est retourné sera de type SqlDataReader. En spécifiant une valeur DataSourceMode de DataSet, les données peuvent être stockées dans un Ensemble de données sur le serveur. Ce mode vous permet d’ajouter des fonctionnalités telles que le tri, le paaging, etc. Si j’avais été la liaison de données le SqlDataSource à un contrôle GridView, j’aurais choisi le mode DataSet. Toutefois, dans le cas d’une DropDownList, le mode DataReader est le bon choix.

> [!NOTE]
> Lors de la mise en cache d’un SqlDataSource ou d’un AccessDataSource, la propriété DataSourceMode doit être définie sur DataSet. Une exception se produira si vous activez la mise en cache avec un DataSourceMode de DataReader.

## <a name="sqldatasource-properties"></a>Propriétés SqlDataSource

Voici quelques-unes des propriétés du contrôle SqlDataSource.

### <a name="cancelselectonnullparameter"></a>AnnulerSlectOnNullParameter

Une valeur Boolean qui précise si une commande sélectionnée est annulée si l’un des paramètres est nul. La valeur par défaut est True.

### <a name="conflictdetection"></a>ConflitDetection

Dans une situation où plusieurs utilisateurs peuvent mettre à jour une source de données en même temps, la propriété ConflictDetection détermine le comportement du contrôle De SqlDataSource. Cette propriété évalue à l’une des valeurs de l’énumération ConflictOptions. Ces valeurs sont **CompareAllValues** et **OverwriteChanges**. Si elle est définie à OverwriteChanges, la dernière personne à écrire des données à la source de données surécrit les modifications précédentes. Toutefois, si la propriété ConflictDetection est définie pour comparerallValues, des paramètres sont créés pour les colonnes retournées par le SelectCommand et des paramètres sont également créés pour contenir les valeurs d’origine dans chacune de ces colonnes permettant à la SqlDataSource de déterminer si les valeurs ont changé ou non depuis l’exécution du SelectCommand.

### <a name="deletecommand"></a>Deletecommand

Définit ou obtient la chaîne SQL utilisée lors de la suppression des lignes de la base de données. Il peut s’agir d’une requête SQL ou d’un nom de procédure stocké.

### <a name="deletecommandtype"></a>SupprimerCommandType

Définit ou obtient le type de commande de suppression, soit une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="deleteparameters"></a>SupprimerParameters

Retourne les paramètres utilisés par le DeleteCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Cette propriété est utilisée pour spécifier le format des paramètres de valeur d’origine dans les cas où la propriété ConflictDetection est définie pour comparerallValues. La valeur {0} par défaut signifie que les paramètres de valeur d’origine porteront le même nom que le paramètre d’origine. En d’autres termes, si le nom de champ @EmployeeIDest EmployeeID, le paramètre de valeur d’origine serait .

### <a name="selectcommand"></a>SelectCommand

Définit ou obtient la chaîne SQL qui est utilisée pour récupérer les données de la base de données. Il peut s’agir d’une requête SQL ou d’un nom de procédure stocké.

### <a name="selectcommandtype"></a>SélectionnezCommandType

Définit ou obtient le type de commande sélectionnée, soit une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="selectparameters"></a>SélectionnezParameters

Retourne les paramètres utilisés par le SelectCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Obtient ou définit le nom d’un paramètre de procédure stocké qui est utilisé lors du tri des données récupérées par le contrôle de source de données. Valable uniquement lorsque SelectCommandType est configuré sur StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency (SqlCacheDependency)

Une chaîne délimitée semi-colon spécifiant les bases de données et les tables utilisées dans une dépendance à la cache SQL Server. (Les dépendances des caches SQL seront discutées dans un module ultérieur.)

### <a name="updatecommand"></a>Updatecommand

Définit ou obtient la chaîne SQL qui est utilisée lors de la mise à jour des données dans la base de données. Il peut s’agir d’une requête SQL ou d’un nom de procédure stocké.

### <a name="updatecommandtype"></a>Mise à jourCommandType

Définit ou obtient le type de commande de mise à jour, soit une requête SQL (texte) ou une procédure stockée (StoredProcedure).

### <a name="updateparameters"></a>Mise à jourParameters

Retourne les paramètres utilisés par le UpdateCommand de l’objet SqlDataSourceView associé au contrôle SqlDataSource.

## <a name="the-accessdatasource-control"></a>Le contrôle AccessDataSource

Le contrôle AccessDataSource provient de la classe SqlDataSource et est utilisé pour se lier de données à une base de données Microsoft Access. La propriété ConnectionString pour le contrôle AccessDataSource est une propriété lue uniquement. Au lieu d’utiliser la propriété ConnectionString, la propriété DataFile est utilisée pour indiquer la base de données d’accès comme indiqué ci-dessous.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource définira toujours le ProviderName de la base SqlDataSource à System.Data.OleDb et se connecte à la base de données à l’aide du fournisseur Microsoft.Jet.OLEDB.4.0 OLE DB. Vous ne pouvez pas utiliser le contrôle AccessDataSource pour vous connecter à une base de données d’accès protégée par mot de passe. Si vous devez vous connecter à une base de données protégée par mot de passe, vous devez utiliser le contrôle SqlDataSource.

> [!NOTE]
> Les bases de données d’accès stockées\_dans le site Web doivent être placées dans l’annuaire App Data. ASP.NET ne permet pas de parcourir les fichiers de cet annuaire. Vous devrez accorder le compte de processus Lire\_et écrire des autorisations à l’annuaire App Data lors de l’utilisation des bases de données Access.

## <a name="the-xmldatasource-control"></a>Le contrôle XmlDataSource

Le XmlDataSource est utilisé pour lier les données XML aux contrôles liés aux données. Vous pouvez vous lier à un fichier XML à l’aide de la propriété DataFile ou vous pouvez vous lier à une chaîne XML à l’aide de la propriété Data. Le XmlDataSource expose les attributs XML comme champs liants. Dans les cas où vous devez vous lier à des valeurs qui ne sont pas représentées comme attributs, vous devrez utiliser une transformation XSL. Vous pouvez également utiliser les expressions XPath pour filtrer les données XML.

Considérez le fichier XML suivant :

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Notez que le XmlDataSource utilise une propriété XPath de &lt;Personnes&gt; */ Personne* afin de filtrer uniquement sur les nœuds Personne. Le DropDownList se lie ensuite à l’attribut LastName à l’aide de la propriété DataTextField.

Bien que le contrôle XmlDataSource soit principalement utilisé pour lier les données aux données XML lue uniquement, il est possible de modifier le fichier de données XML. Notez que dans de tels cas, l’insertion automatique, la mise à jour et la suppression d’informations dans le fichier XML ne se produisent pas automatiquement comme il le fait avec d’autres contrôles de source de données. Au lieu de cela, vous devrez écrire du code pour modifier manuellement les données en utilisant les méthodes suivantes du contrôle XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument (en anglais)

Récupère un objet XmlDocument contenant le code XML récupéré par le XmlDataSource.

### <a name="save"></a>Enregistrer

Enregistre le XmlDocument en mémoire à la source de données.

Il est important de se rendre compte que la méthode Save ne fonctionnera que lorsque les deux conditions suivantes seront remplies :

1. Le XmlDataSource utilise la propriété DataFile pour se lier à un fichier XML au lieu de la propriété Data pour lier aux données XML en mémoire.
2. Aucune transformation n’est spécifiée via la propriété Transform ou TransformFile.

Notez également que la méthode Save peut produire des résultats inattendus lorsqu’elle est appelée par plusieurs utilisateurs simultanément.

## <a name="the-objectdatasource-control"></a>Le contrôle ObjectDataSource

Les contrôles de source de données que nous avons couverts jusqu’à présent sont d’excellents choix pour les applications à deux niveaux où le contrôle de source de données communique directement au magasin de données. Cependant, de nombreuses applications réelles sont des applications à plusieurs niveaux où un contrôle de source de données peut avoir besoin de communiquer à un objet d’affaires qui, à son tour, communique avec la couche de données. Dans ces situations, l’ObjectDataSource remplit bien la facture. L’ObjectDataSource fonctionne en conjonction avec un objet source. Le contrôle ObjectDataSource créera une instance de l’objet source, appellera la méthode spécifiée et éliminera l’instance de l’objet dans le cadre d’une seule demande, si votre objet a des méthodes d’instance au lieu de méthodes statiques (Partage dans Visual Basic). Par conséquent, votre objet doit être apatride. Autrement dit, votre objet doit acquérir et libérer toutes les ressources nécessaires dans le cadre d’une seule demande. Vous pouvez contrôler la façon dont l’objet source est créé en manipulant l’événement ObjectCreating du contrôle ObjectDataSource. Vous pouvez créer une instance de l’objet source, puis définir la propriété ObjectInstance de la classe ObjectDataSourceEventArgs à cette instance. Le contrôle ObjectDataSource utilisera l’instance qui est créée dans l’événement ObjectCreating au lieu de créer une instance par lui-même.

Si l’objet source d’un contrôle ObjectDataSource expose les méthodes statiques publiques (partagées dans Visual Basic) qui peuvent être appelées pour récupérer et modifier les données, un contrôle ObjectDataSource appellera ces méthodes directement. Si un contrôle ObjectDataSource doit créer une instance de l’objet source afin de faire des appels de méthode, l’objet doit inclure un constructeur public qui ne prend aucun paramètre. Le contrôle ObjectDataSource appellera ce constructeur lorsqu’il créera une nouvelle instance de l’objet source.

Si l’objet source ne contient pas un constructeur public sans paramètres, vous pouvez créer une instance de l’objet source qui sera utilisé par le contrôle ObjectDataSource dans l’événement ObjectData Source.

## <a name="specifying-object-methods"></a>Spécifier les méthodes des objets

L’objet source d’un contrôle ObjectDataSource peut contenir n’importe quel nombre de méthodes qui sont utilisées pour sélectionner, insérer, mettre à jour ou supprimer des données. Ces méthodes sont appelées par le contrôle ObjectDataSource basé sur le nom de la méthode, comme indiqué en utilisant soit le SelectMethod, InsertMethod, UpdateMethod, ou Supprimer la propriétéMethod du contrôle ObjectDataSource. L’objet source peut également inclure une méthode SelectCount en option, qui est identifiée par le contrôle ObjectDataSource à l’aide de la propriété SelectCountMethod, qui renvoie le nombre total d’objets à la source de données. Le contrôle ObjectDataSource appellera la méthode SelectCount après qu’une méthode Select a été appelée pour récupérer le nombre total d’enregistrements à la source de données pour une utilisation lors de la recherche de frais.

## <a name="lab-using-data-source-controls"></a>Laboratoire utilisant des contrôles de source de données

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Exercice 1 - Affichage des données avec le contrôle SqlDataSource

L’exercice suivant utilise le contrôle SqlDataSource pour se connecter à la base de données Northwind. Il suppose que vous avez accès à la base de données Northwind sur une instance SQL Server 2000.

1. Créez un site web ASP.NET.
2. Ajoutez un nouveau fichier web.config.

    1. Cliquez à droite sur le projet dans Solution Explorer et cliquez sur Ajouter un nouvel article.
    2. Choisissez le fichier de configuration Web dans la liste des modèles et cliquez sur Ajouter.
3. Modifier &lt;la section&gt; connectionStrings comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Passez à la vue de code et ajoutez un attribut &lt;ConnectionString et un&gt; attribut SelectCommand au contrôle asp:SqlDataSource comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. De la vue design, ajoutez un nouveau contrôle GridView.
6. Parmi le Dropdown Choose Data Source dans le menu GridView Tasks, choisissez SqlDataSource1.
7. Cliquez à droite sur Default.aspx et choisissez Afficher dans le navigateur dans le menu. Cliquez oui lorsqu’on vous demande d’enregistrer.
8. Le GridView affiche les données du tableau produits.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Exercice 2 - Modification des données avec le contrôle SqlDataSource

L’exercice suivant montre comment lier un contrôle DropDownList à l’aide de la syntaxe déclarative et vous permet de modifier les données présentées dans le contrôle DropDownList.

1. Dans la vue de conception, supprimez le contrôle GridView de Default.aspx. 

    **Important**: Laissez le contrôle De SqlDataSource sur la page.
2. Ajoutez un contrôle DropDownList à Default.aspx.
3. Passez à la vue Source.
4. Ajoutez un attribut DataSourceId, DataTextField et DataValueField au &lt;&gt; contrôle asp:DropDownList comme suit : 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Enregistrez Default.aspx et affichez-le dans le navigateur. Notez que la DropDownList contient tous les produits de la base de données Northwind.
6. Fermez le navigateur.
7. Dans la vue Source de Default.aspx, ajoutez un nouveau contrôle TextBox en dessous du contrôle DropDownList. Modifier la propriété ID de la TextBox en txtProductName.
8. Sous le contrôle de TextBox, ajoutez un nouveau contrôle bouton. Modifier la propriété ID du bouton à btnUpdate et la propriété textuelle pour mettre à **jour le nom du produit**.
9. Dans la vue Source de Default.aspx, ajoutez une propriété UpdateCommand et deux nouveaux UpdateParameters à l’étiquette SqlDataSource comme suit: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Notez qu’il existe deux paramètres de mise à jour (ProductName et ProductID) ajoutés dans ce code. Ces paramètres sont cartographiés sur la propriété Text de la textbox txtProductName et la propriété SelectedValue de la ddlProducts DropDownList.
10. Passez à la vue design et double-cliquez sur le contrôle Bouton pour ajouter un gestionnaire d’événement.
11. Ajoutez le code suivant au code\_de clic btnUpdate : 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Cliquez à droite sur Default.aspx et choisissez de le voir dans le navigateur. Cliquez oui lorsqu’on vous demande d’enregistrer toutes les modifications.
13. ASP.NET 2.0 classes partielles permettent la compilation à l’heure de fonctionnement. Il n’est pas nécessaire de créer une application afin de voir les modifications de code prendre effet.
14. Sélectionnez un produit de la DropDownList.
15. Entrez un nouveau nom pour le produit sélectionné dans la TextBox, puis cliquez sur le bouton Mise à jour.
16. Le nom du produit est mis à jour dans la base de données.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Exercice 3 à l’aide du contrôle ObjectDataSource

Cet exercice démontrera comment utiliser le contrôle ObjectDataSource et un objet source pour interagir avec la base de données Northwind.

1. Cliquez à droite sur le projet dans Solution Explorer et cliquez sur Ajouter un nouvel article.
2. Sélectionnez le formulaire Web dans la liste des modèles. Modifier le nom pour object.aspx et cliquez sur Ajouter.
3. Cliquez à droite sur le projet dans Solution Explorer et cliquez sur Ajouter un nouvel article.
4. Sélectionnez classe dans la liste des modèles. Modifier le nom de la classe pour NorthwindData.cs et cliquez sur Ajouter.
5. Cliquez oui lorsqu’on vous demande\_d’ajouter la classe au dossier App Code.
6. Ajoutez le code suivant au fichier NorthwindData.cs : 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Ajoutez le code suivant à la vue Source de object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Enregistrez tous les fichiers et naviguez sur object.aspx.
9. Interagissez avec l’interface en visualisant les détails, en éditant les employés, en ajoutant des employés et en supprimant les employés.
