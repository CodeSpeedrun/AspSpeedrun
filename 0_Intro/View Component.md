# ASP.NET Core: Composants de Vue

## Introduction aux Composants de Vue

Les Composants de Vue dans ASP.NET Core sont similaires aux vues partielles, mais ils sont plus puissants et flexibles. Ils vous permettent d'encapsuler une partie de la logique de l'interface utilisateur (UI) et de la réutiliser dans plusieurs vues. Les Composants de Vue sont comme de mini-contrôleurs pour le rendu d'une partie d'une vue, ce qui en fait un excellent choix pour implémenter des éléments d'interface utilisateur réutilisables avec une logique associée.

## Fondamentaux des Composants de Vue

Les Composants de Vue se composent de deux parties : une classe C# qui hérite de `ViewComponent` et un fichier de vue Razor correspondant.

### Création d'une Classe de Composant de Vue

Commençons par créer une classe de Composant de Vue nommée `ComposantVuePersonnalise`.

```csharp
using Microsoft.AspNetCore.Mvc;

public class ComposantVuePersonnalise : ViewComponent
{
    public IViewComponentResult Invoke()
    {
        return View();
    }
}
```

Dans cet exemple, `ComposantVuePersonnalise` hérite de `ViewComponent` et fournit une méthode `Invoke()`. Cette méthode retourne un `IViewComponentResult`, qui représente généralement le résultat du rendu d'une vue.

### Création d'une Vue de Composant de Vue

Ensuite, nous devons créer un fichier de vue Razor correspondant à notre Composant de Vue. Nous le nommerons `Default.cshtml` et le placerons dans le dossier `Views/Shared/Components/ComposantVuePersonnalise`.

```html
<!-- Default.cshtml -->
<div>
    <!-- L'interface utilisateur du Composant de Vue va ici -->
</div>
```

Cette vue Razor sera utilisée pour rendre l'interface utilisateur de notre Composant de Vue.

### Utilisation du Composant de Vue dans une Vue

Pour utiliser le Composant de Vue dans une vue, nous pouvons l'appeler en utilisant la méthode `Component.InvokeAsync()` en passant le nom du Composant de Vue.

```html
@await Component.InvokeAsync("ComposantVuePersonnalise")
```

Cela rendra l'interface utilisateur définie dans la vue Razor du Composant de Vue.

## Passage de Données aux Composants de Vue

Les Composants de Vue peuvent accepter des paramètres comme des méthodes classiques. Cela nous permet de transmettre des données au Composant de Vue à partir de la vue parent ou du contrôleur.

### Exemple : Passage de Données à un Composant de Vue

Modifions notre `ComposantVuePersonnalise` pour accepter un paramètre.

```csharp
public class ComposantVuePersonnalise : ViewComponent
{
    public IViewComponentResult Invoke(string message)
    {
        ViewData["Message"] = message;
        return View();
    }
}
```

Maintenant, nous pouvons passer un message au Composant de Vue lors de son appel.

```html
@await Component.InvokeAsync("ComposantVuePersonnalise", new { message = "Bonjour depuis le Composant de Vue !" })
```

## Conclusion

Les Composants de Vue dans ASP.NET Core fournissent un mécanisme puissant pour encapsuler la logique de l'interface utilisateur et la rendre réutilisable dans différentes parties de votre application. En créant des Composants de Vue personnalisés et en utilisant des paramètres, vous pouvez créer des éléments d'interface utilisateur flexibles et modulaires qui améliorent la maintenabilité et la scalabilité de vos applications web.