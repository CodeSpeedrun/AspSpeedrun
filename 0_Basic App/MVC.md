## Introduction à ASP.NET Core MVC

ASP.NET Core MVC est un framework puissant pour le développement d'applications web dynamiques utilisant le modèle d'architecture Modèle-Vue-Contrôleur (MVC). Il offre un environnement robuste, flexible et performant pour le développement d'applications web.

### Architecture MVC

MVC divise une application en trois composants principaux :

1. **Modèle**: Représente les données de l'application et la logique métier.
2. **Vue**: Affiche les données à l'utilisateur et gère l'interaction utilisateur.
3. **Contrôleur**: Traite l'entrée utilisateur, interagit avec le modèle et sélectionne la vue à rendre.

### Fonctionnalités d'ASP.NET Core MVC

#### Routage

Le routage dans ASP.NET Core MVC définit comment les URL sont mappées aux actions de contrôleur. Il fournit un système de routage flexible qui permet aux développeurs de définir des routes personnalisées pour traiter les demandes.

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(
            name: "default",
            pattern: "{controller=Accueil}/{action=Index}/{id?}");
    });
}
```

Dans l'exemple ci-dessus, la méthode `MapControllerRoute` est utilisée pour définir une route par défaut où le contrôleur est défini sur `Accueil`, l'action est définie sur `Index`, et un paramètre `id` facultatif peut être passé.

#### Contrôleurs

Les contrôleurs sont le cœur des applications ASP.NET Core MVC. Ils gèrent les demandes des utilisateurs, exécutent la logique appropriée et renvoient des réponses au client.

```csharp
public class AccueilController : Controller
{
    public IActionResult Index()
    {
        return View();
    }
}
```

Dans l'exemple ci-dessus, `AccueilController` est une classe de contrôleur avec une méthode d'action `Index` qui renvoie un résultat de vue.

#### Vues

Les vues sont responsables du rendu de l'interface utilisateur en fonction des données fournies par le contrôleur. Elles sont généralement écrites en utilisant la syntaxe Razor, qui combine le balisage HTML avec du code C#.

```html
@model IEnumerable<Produit>

@foreach (var produit in Model)
{
    <div>
        <h2>@produit.Nom</h2>
        <p>@produit.Description</p>
    </div>
}
```

Dans le code de la vue ci-dessus, la directive `@model` spécifie le type du modèle passé depuis le contrôleur, et la boucle `@foreach` itère sur la collection de produits pour générer le balisage HTML.

#### Injection de dépendances

ASP.NET Core MVC prend en charge l'injection de dépendances, permettant aux développeurs d'injecter des dépendances dans les contrôleurs, les vues et d'autres composants.

```csharp
public class AccueilController : Controller
{
    private readonly ILogger<AccueilController> _logger;

    public AccueilController(ILogger<AccueilController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        _logger.LogInformation("Action Index exécutée.");
        return View();
    }
}
```

Dans l'exemple ci-dessus, `ILogger` est injecté dans le `AccueilController`, permettant l'enregistrement de journalisation à l'intérieur de l'action du contrôleur.

### Conclusion

ASP.NET Core MVC offre un cadre robuste pour le développement d'applications web modernes. En exploitant le modèle MVC, les développeurs peuvent créer des bases de code bien structurées et faciles à maintenir. Avec des fonctionnalités telles que le routage, les contrôleurs, les vues et l'injection de dépendances, ASP.NET Core MVC offre une flexibilité et une évolutivité pour le développement d'applications web.




# Complément aux Notes sur ASP.NET Core MVC

## Modèle-Vue-Contrôleur (MVC) en Profondeur

### Modèle

Le modèle représente la couche de données de l'application. Il encapsule la logique métier, les données et les règles de validation. Dans une application ASP.NET Core MVC, le modèle peut être représenté par des classes de modèle simples ou des ORM (Object-Relational Mapping) tels qu'Entity Framework Core.

Exemple de classe modèle :

```csharp
public class Produit
{
    public int Id { get; set; }
    public string Nom { get; set; }
    public string Description { get; set; }
    public decimal Prix { get; set; }
}
```

### Vue

La vue est responsable de l'affichage des données au client. Elle utilise des fichiers de modèle (fichiers .cshtml dans ASP.NET Core MVC) pour générer le contenu HTML à envoyer au navigateur. Les vues peuvent être fortement typées, ce qui signifie qu'elles reçoivent un modèle spécifique du contrôleur pour l'affichage des données.

Exemple de vue fortement typée :

```html
@model MonApplication.Models.Produit

<h2>@Model.Nom</h2>
<p>@Model.Description</p>
<p>Prix: @Model.Prix</p>
```

### Contrôleur

Le contrôleur agit comme un intermédiaire entre le modèle et la vue. Il traite les demandes HTTP, récupère les données du modèle et les passe à la vue appropriée pour l'affichage. Les contrôleurs contiennent des actions, qui sont des méthodes qui répondent à des requêtes spécifiques.

Exemple d'action de contrôleur :

```csharp
public class ProduitController : Controller
{
    private readonly ApplicationDbContext _context;

    public ProduitController(ApplicationDbContext context)
    {
        _context = context;
    }

    public IActionResult Details(int id)
    {
        var produit = _context.Produits.FirstOrDefault(p => p.Id == id);
        if (produit == null)
        {
            return NotFound();
        }
        return View(produit);
    }
}
```

### Cycle de Vie de la Requête

Lorsqu'une requête est reçue par l'application ASP.NET Core MVC, elle passe par plusieurs étapes, y compris le routage, le traitement par le contrôleur, la récupération des données du modèle et le rendu de la vue. Comprendre le cycle de vie de la requête est essentiel pour diagnostiquer les problèmes et optimiser les performances de l'application.

### Liaisons de Modèle

Les liaisons de modèle sont utilisées pour mapper les données reçues dans une requête HTTP aux propriétés du modèle. ASP.NET Core MVC fournit différentes stratégies de liaison de modèle, y compris la liaison de modèle par défaut, la liaison de modèle explicite et la liaison de modèle personnalisée.

### Validation des Données

La validation des données est essentielle pour garantir l'intégrité des données dans une application. ASP.NET Core MVC propose un système de validation intégré qui permet de définir des règles de validation pour les propriétés du modèle et de valider automatiquement les données reçues dans une requête.

### Filtres d'Action

Les filtres d'action permettent d'exécuter du code avant ou après l'exécution d'une action de contrôleur. Ils peuvent être utilisés pour l'authentification, l'autorisation, la mise en cache, la journalisation et d'autres tâches transversales à l'application.

### Conclusion

En comprenant en profondeur les concepts de Modèle-Vue-Contrôleur (MVC) dans ASP.NET Core, les développeurs peuvent créer des applications web robustes, évolutives et faciles à maintenir. En combinant le modèle, la vue et le contrôleur de manière efficace, il est possible de développer des applications qui offrent une expérience utilisateur exceptionnelle et répondent aux besoins métier les plus complexes.