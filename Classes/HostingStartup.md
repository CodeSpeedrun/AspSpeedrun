# ASP.NET Core : Compréhension de l'attribut HostingStartup

L'attribut `HostingStartup` joue un rôle essentiel dans la configuration des services et des middleware lors du démarrage d'une application ASP.NET Core. Il permet de spécifier des classes qui seront exécutées automatiquement au démarrage de l'application, facilitant ainsi la configuration initiale de celle-ci. Cet attribut est souvent utilisé pour ajouter des fonctionnalités supplémentaires, des services, ou des middleware à l'application.

## Vue d'ensemble

Dans le contexte d'ASP.NET Core, l'attribut `HostingStartup` est généralement utilisé dans le fichier `AssemblyInfo.cs` d'un projet pour indiquer les types à exécuter lors du démarrage de l'application. Ces types doivent implémenter l'interface `IHostingStartup`. L'utilisation de cet attribut permet de définir des tâches d'initialisation qui doivent être effectuées avant le traitement des requêtes HTTP.

## Syntaxe

```csharp
[assembly: HostingStartup(typeof(MonDemarragePersonnalise))]
```

Dans cette syntaxe, `MonDemarragePersonnalise` est une classe qui implémente l'interface `IHostingStartup`. Cette interface comporte une méthode, `Configure`, qui permet de configurer les services et les middleware de l'application.

## Fonctionnement

Lorsque l'application ASP.NET Core démarre, elle recherche les types marqués avec l'attribut `HostingStartup` et exécute les méthodes `Configure` de ces types dans l'ordre spécifié. Ces méthodes sont responsables de la configuration des services et des middleware nécessaires à l'application. Cette approche permet de découpler la configuration initiale de l'application de son code principal, ce qui la rend plus modulaire et facilement extensible.

## Exemple

Prenons un exemple où nous souhaitons ajouter un middleware personnalisé lors du démarrage de l'application en utilisant l'attribut `HostingStartup`.

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;

public class MonDemarragePersonnalise : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        builder.Configure(app =>
        {
            app.UseMiddleware<MonMiddlewarePersonnalise>();
        });
    }
}
```

Dans cet exemple, `MonDemarragePersonnalise` est une classe qui implémente l'interface `IHostingStartup`. La méthode `Configure` de cette classe configure un middleware personnalisé (`MonMiddlewarePersonnalise`) à utiliser par l'application.

## Utilisation

Pour utiliser l'attribut `HostingStartup` dans votre application ASP.NET Core, suivez les étapes suivantes :

1. Implémentez une classe qui implémente l'interface `IHostingStartup`.
2. Dans cette classe, implémentez la méthode `Configure` pour configurer les services et les middleware de l'application.
3. Ajoutez l'attribut `HostingStartup` dans le fichier `AssemblyInfo.cs` de votre projet, en spécifiant le type de la classe de démarrage personnalisée.

```csharp
[assembly: HostingStartup(typeof(MonDemarragePersonnalise))]
```

En suivant ces étapes, les méthodes de configuration spécifiées dans la classe `MonDemarragePersonnalise` seront exécutées automatiquement au démarrage de l'application.

```csharp
namespace Microsoft.AspNetCore.Hosting {
    //
    // Summary:
    //     Marker attribute indicating an implementation of Microsoft.AspNetCore.Hosting.IHostingStartup
    //     that will be loaded and executed when building an Microsoft.AspNetCore.Hosting.IWebHost.
    [AttributeUsage(AttributeTargets.Assembly, Inherited = false, AllowMultiple = true)]
    public sealed class HostingStartupAttribute : Attribute {
        //
        // Summary:
        //     Constructs the Microsoft.AspNetCore.Hosting.HostingStartupAttribute with the
        //     specified type.
        //
        // Parameters:
        //   hostingStartupType:
        //     A type that implements Microsoft.AspNetCore.Hosting.IHostingStartup.
        public HostingStartupAttribute(Type hostingStartupType);

        //
        // Summary:
        //     The implementation of Microsoft.AspNetCore.Hosting.IHostingStartup that should
        //     be loaded when starting an application.
        public Type HostingStartupType { get; }
    }
}
```

## Conclusion

L'attribut `HostingStartup` est un outil puissant pour la configuration initiale des applications ASP.NET Core. En permettant l'exécution automatique de tâches de démarrage, il simplifie le processus de configuration et rend l'application plus modulaire et extensible. En comprenant son fonctionnement et en l'utilisant correctement, les développeurs peuvent créer des applications robustes et faciles à maintenir.