Program.cs
```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

namespace BethanysPieShop {
    public class Program {
        public static void Main(string[] args) {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder => {
                    webBuilder.UseStartup<Startup>();
                });
    }
}
```

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

namespace BethanysPieShop {
    public class Program {
        public static void Main(string[] args) {
            // Create a host builder with the given command-line arguments
            IHostBuilder hostBuilder = CreateHostBuilder(args);

            // Build the host
            IHost host = hostBuilder.Build();

            // Run the host
            host.Run();
        }

        // Define a method to create a host builder
        public static IHostBuilder CreateHostBuilder(string[] args) {
            // Create a default host builder with the given command-line arguments
            IHostBuilder hostBuilder = Host.CreateDefaultBuilder(args);

            // Configure the web host to use the Startup class for configuration
            hostBuilder.ConfigureWebHostDefaults(ConfigureWebHost);

            return hostBuilder;
        }

        // Define a method to configure the web host
        public static void ConfigureWebHost(IWebHostBuilder webBuilder) {
            // Use the Startup class for configuring the web host
            webBuilder.UseStartup<Startup>();
        }
    }
}

```
# Notes sur ASP.NET Core 3.0

Dans ce fichier markdown, nous allons explorer la structure fondamentale d'une application ASP.NET Core 3.0. Nous plongerons dans la classe `Program` et discuterons en détail de ses composants.

## La Classe `Program`

La classe `Program` sert de point d'entrée pour l'application ASP.NET Core. Elle contient la méthode `Main`, qui est responsable de l'amorçage de l'application et de la création de l'hôte.

### Méthode Main

```csharp
public class MonApplication {
    public static void Main(string[] args) {
        ConstruireEtExécuterHôte(args);
    }

    public static void ConstruireEtExécuterHôte(string[] args) {
        CréerConstructeurHôte(args).Build().Run();
    }

    public static IHostBuilder CréerConstructeurHôte(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder => {
                webBuilder.UseStartup<Startup>();
            });
}
```

La méthode `Main` est le point d'entrée de l'application. Elle appelle la méthode `ConstruireEtExécuterHôte`, qui encapsule la logique de création et d'exécution de l'hôte.

### Méthode ConstruireEtExécuterHôte

La méthode `ConstruireEtExécuterHôte` est responsable de la création et de l'exécution de l'hôte. Elle appelle la méthode `CréerConstructeurHôte` pour configurer le constructeur d'hôte, construit l'hôte, puis l'exécute.

### Méthode CréerConstructeurHôte

La méthode `CréerConstructeurHôte` configure le constructeur d'hôte. Elle met en place des configurations par défaut en utilisant `Host.CreateDefaultBuilder`. Dans la méthode `ConfigureWebHostDefaults`, elle spécifie la classe de démarrage (`Startup`) à utiliser pour configurer les services et les middlewares de l'application.

## Conclusion

Comprendre la structure de la classe `Program` est crucial pour l'amorçage d'une application ASP.NET Core. En saisissant les subtilités des méthodes `Main`, `ConstruireEtExécuterHôte` et `CréerConstructeurHôte`, les développeurs acquièrent une vision de la façon dont l'hôte de l'application est configuré et démarré.

Dans les sections suivantes, nous approfondirons des sujets tels que la configuration des middlewares, l'injection de dépendances et le routage, en nous appuyant sur les connaissances fondamentales fournies dans ce markdown.

## Kestrel
Allons plus en détail sur la manière dont la configuration fournie met en place Kestrel en tant que serveur interne pour l'application ASP.NET Core :

1. **Méthode ConfigureWebHostDefaults**: Cette méthode fait partie de l'interface `IWebHostBuilder` fournie par ASP.NET Core. Elle configure l'hôte web en utilisant des paramètres par défaut. À l'intérieur de cette méthode, `webBuilder.UseStartup<Startup>()` est appelé.

2. **Méthode UseStartup**: Cette méthode spécifie la classe de démarrage (`Startup`) qui configure l'application. La classe `Startup` est l'endroit où vous définissez comment l'application répond aux requêtes HTTP. En spécifiant `Startup` ici, vous indiquez à ASP.NET Core d'utiliser cette classe pour configurer les services et les middlewares de l'application.

3. **Classe Startup**: Dans la classe `Startup`, vous configurez généralement les services pour l'injection de dépendances et les middlewares pour gérer les requêtes HTTP. Les composants de middleware sont ajoutés au pipeline de requêtes, et ce pipeline est exécuté pour chaque requête entrante.

4. **Kestrel**: Kestrel est le serveur web multiplateforme utilisé par les applications ASP.NET Core pour gérer les requêtes HTTP. Lorsque vous créez une application ASP.NET Core, Kestrel est inclus par défaut. Il est optimisé pour servir les applications ASP.NET Core et est souvent utilisé comme serveur interne pendant le développement.

5. **Compatibilité multiplateforme**: L'un des avantages significatifs de Kestrel est sa compatibilité multiplateforme. Il peut s'exécuter sur les environnements Windows, macOS et Linux, ce qui rend les applications ASP.NET Core hautement portables.

En configurant l'application pour utiliser `Startup` avec `webBuilder.UseStartup<Startup>()`, vous configurez Kestrel pour servir votre application ASP.NET Core. Kestrel écoute les requêtes HTTP entrantes et les délègue au pipeline de middleware configuré pour le traitement, permettant à votre application de gérer les requêtes de manière efficace.