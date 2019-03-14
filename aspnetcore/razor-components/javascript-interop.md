---
title: Interopérabilité des composants JavaScript Razor
author: guardrex
description: Découvrez comment appeler des fonctions JavaScript à partir de .NET et .NET méthodes à partir de JavaScript dans les applications Blazor et composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: 9f822fee8990b03ff15ffa9857a2ddd95328a6ce
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050366"
---
# <a name="razor-components-javascript-interop"></a>Interopérabilité des composants JavaScript Razor

Par [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), et [Luke Latham](https://github.com/guardrex)

Une application Razor composants peut appeler des fonctions JavaScript à partir de .NET et .NET méthodes depuis le code JavaScript.

## <a name="invoke-javascript-functions-from-net-methods"></a>Appeler des fonctions JavaScript à partir de méthodes .NET

Il est parfois lorsque le code .NET est requis pour appeler une fonction JavaScript. Par exemple, un appel JavaScript peut exposer des fonctionnalités du navigateur ou fonctionnalités à partir d’une bibliothèque JavaScript à l’application.

Pour appeler en JavaScript à partir de .NET, utilisez la `IJSRuntime` abstraction. Le `InvokeAsync<T>` méthode sur `IJSRuntime` prend un identificateur pour la fonction JavaScript que vous souhaitez appeler avec n’importe quel nombre d’arguments JSON sérialisable. L’identificateur de fonction est relatif à la portée globale (`window`). Si vous souhaitez appeler `window.someScope.someFunction`, l’identificateur est `someScope.someFunction`. Il est inutile pour inscrire la fonction avant qu’elle est appelée. Le type de retour `T` JSON doit également être sérialisable.

Pour les applications ASP.NET Core Razor composants côté serveur :

* Plusieurs demandes d’utilisateur sont traitées par l’application côté serveur. N’appelez pas `JSRuntime.Current` dans un composant à appeler des fonctions JavaScript.
* Injecter le `IJSRuntime` abstraction et l’utilisation de l’objet injecté pour émettre des appels interop JavaScript.

L’exemple suivant est basé sur [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), un décodeur JavaScript expérimental. L’exemple montre comment appeler une fonction JavaScript à partir d’un C# (méthode). La fonction JavaScript accepte un tableau d’octets à partir d’un C# (méthode), décode le tableau et retourne le texte au composant pour l’affichage.

À l’intérieur de la `<head>` élément de *wwwroot/index.HTML*, fournissez une fonction qui utilise `TextDecoder` pour décoder un tableau passé :

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

Code JavaScript, telles que le code indiqué dans l’exemple précédent, permettre également être chargé par un fichier JavaScript avec une référence au fichier de script dans le *wwwroot/index.HTML* fichier.

Le composant suivant :

* Appelle le `ConvertArray` à l’aide de fonction JavaScript `JsRuntime` quand un bouton de composant (**convertir un tableau**) est sélectionné.
* Une fois que la fonction JavaScript est appelée, le tableau passé est converti en une chaîne. La chaîne est retournée au composant pour l’affichage.

```cshtml
@page "/"
@using Microsoft.JSInterop;
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

Pour les applications de Blazor côté client, le `IJSRuntime` abstraction est accessible à partir de `JSRuntime.Current`, qui fait référence à la demande de l’utilisateur actuel. Étant donné qu’un seul utilisateur d’une application de Blazor côté client, à l’aide `JSRuntime.Current` pour appeler un code JavaScript fonctionne normalement. Utilisez uniquement `JSRuntime.Current` dans les applications Blazor côté client.

Dans l’application exemple côté client qui accompagne cette rubrique, les deux fonctions JavaScript sont disponibles pour l’application côté client qui interagissent avec le DOM à l’entrée d’utilisateur et afficher un message d’accueil :

* `showPrompt` &ndash; Génère une invite d’accepter l’entrée d’utilisateur (le nom d’utilisateur) et retourne le nom à l’appelant.
* `displayWelcome` &ndash; Assigne un message d’accueil de l’appelant à un objet DOM avec un `id` de `welcome`.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

Place le `<script>` balise qui référence le fichier JavaScript dans le *wwwroot/index.HTML* fichier :

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=16)]

Ne pas de placer une balise de script dans un fichier de composant, car la balise de script ne peut pas être mis à jour dynamiquement.

Interopérabilité des méthodes .NET avec les fonctions JavaScript en appelant `InvokeAsync<T>` méthode sur `IJSRuntime`.

L’exemple d’application utilise une paire de C# méthodes, `Prompt` et `Display`, pour appeler le `showPrompt` et `displayWelcome` fonctions JavaScript :

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=6-8,14-16)]

Le `IJSRuntime` abstraction est asynchrone pour permettre des scénarios côté serveur. Si l’application s’exécute côté client et que vous souhaitez appeler une fonction JavaScript de manière synchrone, effectuer un downcast à `IJSInProcessRuntime` et appelez `Invoke<T>` à la place. Nous recommandons que la plupart des utilisation d’interop bibliothèques JavaScript les API pour garantir les bibliothèques asynchrones sont disponibles dans tous les scénarios, côté client ou côté serveur.

L’exemple d’application inclut un composant pour illustrer l’interopérabilité JS. Le composant :

* Reçoit l’entrée utilisateur via une invite de JS.
* Retourne le texte au composant pour traitement.
* Appelle une deuxième fonction JS qui interagit avec le DOM pour afficher un message d’accueil.

*Pages/JSInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=1&end=21&highlight=2-3,9-11,13,16-20)]

1. Lorsque `TriggerJsPrompt` est exécutée en sélectionnant le composant **déclencheur JavaScript invite** bouton, le `ExampleJsInterop.Prompt` méthode dans C# code est appelé.
1. Le `Prompt` méthode s’exécute le code JavaScript `showPrompt` fournie dans le *wwwroot/exampleJsInterop.js* fichier.
1. Le `showPrompt` fonction accepte l’entrée d’utilisateur (nom de l’utilisateur), qui est codée en HTML et renvoyée à le `Prompt` (méthode), puis finalement vers le composant. Le composant stocke le nom d’utilisateur dans une variable locale, `name`.
1. La chaîne stockée dans `name` est incorporé dans un message d’accueil, qui est transmis à un deuxième C# (méthode), `ExampleJsInterop.Display`.
1. `Display` appelle une fonction JavaScript, `displayWelcome`, qui restitue le message d’accueil dans une balise d’en-tête.

## <a name="capture-references-to-elements"></a>Capturer les références aux éléments

Certains [JavaScript interop](xref:razor-components/javascript-interop) scénarios requièrent des références aux éléments HTML. Par exemple, une bibliothèque d’interface utilisateur peut nécessiter une référence d’élément pour l’initialisation, ou vous devrez peut-être appeler des API de type de commande sur un élément, telle que `focus` ou `play`.

Vous pouvez capturer des références aux éléments HTML dans un composant en ajoutant un `ref` d’attribut à l’élément HTML et en définissant un champ de type `ElementRef` dont le nom correspond à la valeur de la `ref` attribut.

L’exemple suivant montre la capture d’une référence à l’élément d’entrée de nom d’utilisateur :

```csharp
<input ref="username" ... />

@functions {
    ElementRef username;
}
```

> [!NOTE]
> Faire **pas** utiliser des références de l’élément capturé comme un moyen de remplir le modèle DOM. Cela peut interférer avec le modèle déclaratif de rendu.

Autant que code .NET est concerné, un `ElementRef` est un handle opaque. Le *uniquement* chose que vous pouvez en faire est de transmettre au code JavaScript grâce à l’interopérabilité de JavaScript. Lorsque vous procédez ainsi, le code JavaScript côté reçoit un `HTMLElement` instance, il peut utiliser avec les API DOM normal.

Par exemple, le code suivant définit une méthode d’extension .NET qui permet de définir le focus sur un élément :

*myLib.js*:

```javascript
window.myLib = {
  focusElement : function (element) {
    element.focus();
  }
}
```

*ElementRefExtensions.cs*:

```csharp
using Microsoft.AspNetCore.Blazor;
using Microsoft.JSInterop;
using System.Threading.Tasks;

namespace MyLib
{
    public static class MyLibElementRefExtensions
    {
        public static Task Focus(this ElementRef elementRef)
        {
            return JSRuntime.Current.InvokeAsync<object>("myLib.focusElement", elementRef);
        }
    }
}
```

Vous pouvez désormais vous concentrer entrées dans un de vos composants :

```cshtml
@using MyLib

<input ref="username" />
<button onclick="@SetFocus">Set focus</button>

@functions {
    ElementRef username;

    void SetFocus()
    {
        username.Focus();
    }
}
```

> [!IMPORTANT]
> Le `username` variable est renseignée uniquement une fois que le composant effectue le rendu et sa sortie inclut le `<input>` élément. Si vous essayez de transmettre un non remplie `ElementRef` au code JavaScript, reçoit le code JavaScript `null`. Pour manipuler les références d’élément après que le composant a terminé l’utilisation de rendu (pour définir le focus initial sur un élément) le `OnAfterRenderAsync` ou `OnAfterRender` [méthodes de cycle de vie du composant](xref:razor-components/components#lifecycle-methods).

## <a name="invoke-net-methods-from-javascript-functions"></a>Appeler des méthodes .NET à partir de fonctions JavaScript

### <a name="static-net-method-call"></a>Appel de méthode .NET statique

Pour appeler une méthode .NET statique à partir de JavaScript, utilisez le `DotNet.invokeMethod` ou `DotNet.invokeMethodAsync` fonctions. Passez l’identificateur de la méthode statique que vous souhaitez appeler, le nom de l’assembly contenant la fonction et tous les arguments. Là encore, la version asynchrone est préférable pour prendre en charge des scénarios côté serveur. Pour être appelables à partir de JavaScript, la méthode .NET doit être publique et statique décorée avec `[JSInvokable]`. Par défaut, l’identificateur de méthode est le nom de méthode, mais vous pouvez spécifier un autre identificateur à l’aide de la `JSInvokableAttribute` constructeur. Appel de méthodes génériques ouvertes n’est pas actuellement pris en charge.

L’exemple d’application inclut un C# méthode pour retourner un tableau de `int`s. La méthode est décorée avec le `JSInvokable` attribut.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=47&end=58&highlight=7-11)]

JavaScript pris en charge pour le client appelle le C# méthode .NET.

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-12)]

Lorsque le **méthode statique .NET de déclencheur ReturnArrayAsync** est sélectionné, examinez la sortie de console dans les outils de développement du navigateur web :

```console
Array(4) [ 1, 2, 3, 4 ]
```

La quatrième valeur de tableau est poussée vers le tableau (`data.push(4);`) retourné par `ReturnArrayAsync`.

### <a name="instance-method-call"></a>Appel de méthode d’instance

Vous pouvez également appeler des méthodes d’instance .NET à partir de JavaScript. Pour appeler une méthode d’instance .NET à partir de JavaScript, tout d’abord passer l’instance de .NET à JavaScript en l’encapsulant dans un `DotNetObjectRef` instance. L’instance de .NET est passé par référence à JavaScript, et vous pouvez appeler des méthodes d’instance de .NET sur l’instance à l’aide de la `invokeMethod` ou `invokeMethodAsync` fonctions. L’instance de .NET peut également être passé en tant qu’argument lors de l’appel d’autres méthodes de .NET à partir de JavaScript.

> [!NOTE]
> L’exemple d’application consigne les messages dans la console du côté client. Pour les exemples suivants, illustrées dans l’exemple d’application, examinez la sortie de la console du navigateur dans les outils de développement du navigateur.

Lorsque le **méthode d’instance déclencheur .NET HelloHelper.SayHello** bouton est sélectionné, `ExampleJsInterop.CallHelloHelperSayHello` est appelée et transmet un nom, `Blazor`, à la méthode.

*Pages/JsInterop.cshtml*:

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?start=60&end=69&highlight=8)]

`CallHelloHelperSayHello` appelle la fonction JavaScript `sayHello` avec une nouvelle instance de `HelloHelper`.

*JsInteropClasses/ExampleJsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=19-25)]

*wwwroot/exampleJsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-17)]

Le nom est passé à `HelloHelper`du constructeur, ce qui affecte le `HelloHelper.Name` propriété. Lorsque la fonction JavaScript `sayHello` est exécutée, `HelloHelper.SayHello` retourne le `Hello, {Name}!` message, ce qui est écrit dans la console par la fonction JavaScript.

*JsInteropClasses/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

Console de sortie dans les outils de développement du navigateur web :

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a>Partagez du code interop dans une bibliothèque de classes de composant de Razor

Code interop JavaScript peut être inclus dans une bibliothèque de classes de composant de Razor (`dotnet new blazorlib`), qui vous permet de partager le code dans un package NuGet.

La bibliothèque de classes Razor composant gère les ressources JavaScript incorporation dans l’assembly généré. Les fichiers JavaScript sont placés dans le *wwwroot* dossier et les outils s’occupe de l’incorporation de ces ressources lors de la génération de la bibliothèque.

Le package NuGet intégré est référencé dans le fichier de projet de l’application, tout comme n’importe quel package NuGet normal est référencé. Une fois que l’application a été restaurée, code d’application peut appeler dans JavaScript comme s’il s’agissait C#.
