### L'injection de dépendances ASP.NET Core

L'injection de dépendances (DI) est un concept fondamental dans ASP.NET Core, facilitant la séparation des composants et améliorant la testabilité et la maintenabilité des applications. Dans ASP.NET Core, l'injecteur de dépendances repose principalement sur trois méthodes : `AddTransient`, `AddSingleton` et `AddScoped`.

#### AddTransient
`AddTransient` enregistre un service avec une durée de vie transitoire. Cela signifie qu'une nouvelle instance du service sera créée à chaque fois qu'elle est demandée.

```csharp
services.AddTransient<IService, ServiceTransitoire>();
```

- **Durée de vie transitoire** : Chaque fois que `IService` est demandé, une nouvelle instance de `ServiceTransitoire` est créée.
- **Utilisation** : Adapté aux services légers et sans état ou aux services avec des durées de vie courtes.

#### AddSingleton
`AddSingleton` enregistre un service avec une durée de vie singleton. Une seule instance du service est créée et partagée dans toute l'application.

```csharp
services.AddSingleton<IService, ServiceSingleton>();
```

- **Durée de vie Singleton** : Une seule instance de `ServiceSingleton` est créée et partagée dans toute l'application.
- **Utilisation** : Idéal pour les services sans état ou destinés à être utilisés comme ressources partagées dans toute l'application.

#### AddScoped
`AddScoped` enregistre un service avec une durée de vie de portée. L'instance du service est créée une fois par requête client (requête HTTP dans le cas d'ASP.NET Core).

```csharp
services.AddScoped<IService, ServiceDePortée>();
```

- **Durée de vie de portée** : Une seule instance de `ServiceDePortée` est créée pour chaque requête client, et elle est partagée entre les composants dans la même requête HTTP.
- **Utilisation** : Adapté aux services nécessitant un état dans le cadre d'une requête HTTP.

### Informations Additionnelles

#### Choix de la bonne durée de vie

- **Transitoire vs Singleton vs Portée** : Considérez la nature de votre service et ses besoins en termes de durée de vie. Les services transitoires sont légers et sont créés pour chaque requête, tandis que les singletons sont partagés dans toute l'application. Les services de portée se situent entre les deux, définissant une portée pour une requête particulière.

#### Durées de vie des services dans les applications Web

- **Services transitoires** : Utilisés pour des opérations sans état et ne nécessitant pas de persistance entre les requêtes.
- **Services singleton** : Utiles pour le cache, les journaux ou autres ressources partagées nécessitant de maintenir un état tout au long de la durée de vie de l'application.
- **Services de portée** : Typiquement utilisés pour la gestion de l'état par requête ou les contextes de base de données où les données doivent être limitées à une requête HTTP particulière.

#### Exemple

Prenons un exemple d'utilisation de l'injection de dépendances dans une application web ASP.NET Core :

```csharp
public interface IServiceDePanier
{
    void AjouterArticleAuPanier(string idArticle, int quantité);
    IEnumerable<ArticlePanier> ObtenirArticlesDuPanier();
}

public class ServiceDePanier : IServiceDePanier
{
    private readonly List<ArticlePanier> _articlesPanier;

    public ServiceDePanier()
    {
        _articlesPanier = new List<ArticlePanier>();
    }

    public void AjouterArticleAuPanier(string idArticle, int quantité)
    {
        // Ajouter un article au panier
        _articlesPanier.Add(new ArticlePanier(idArticle, quantité));
    }

    public IEnumerable<ArticlePanier> ObtenirArticlesDuPanier()
    {
        // Retourner les articles du panier
        return _articlesPanier;
    }
}

// Dans Startup.cs
public void ConfigureServices(IServiceCollection services)
{
    // Enregistrer le service de panier avec une durée de vie de portée
    services.AddScoped<IServiceDePanier, ServiceDePanier>();
}
```

Dans cet exemple, `ServiceDePanier` implémente `IServiceDePanier`, qui définit des méthodes pour ajouter des articles à un panier et récupérer les articles du panier. Le service est enregistré avec une durée de vie de portée, garantissant que chaque requête HTTP reçoit sa propre instance du service de panier. Cela permet une séparation adéquate des données entre différentes requêtes client.