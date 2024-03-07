# Configuration et Mise en Place d'une Application ASP.NET Core 3.0

Dans ce guide, nous allons explorer la configuration et la mise en place d'une application ASP.NET Core 3.0, en mettant l'accent sur la classe `Startup`. Cette classe joue un rôle central dans la configuration des services, des middleware et du pipeline de traitement des requêtes.

## La Classe `Startup`

```csharp
using PieShop.Models;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace PieShop {
    public class MyStartup {
        public MyStartup(IConfiguration configuration) {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // ConfigureServices: Méthode pour ajouter des services au conteneur.
        public void ConfigureMesServices(IServiceCollection services) {
            services.AddControllersWithViews();
            services.AddScoped<IPieRepository, MockPieRepository>();
            services.AddScoped<ICategoryRepository, MockCategoryRepository>();
        }

        // Configure: Méthode pour configurer le pipeline de requêtes HTTP.
        public void ConfigureMonApp(IApplicationBuilder app, IWebHostEnvironment env) {
            if (env.IsDevelopment()) {
                app.UseDeveloperExceptionPage(); // Page d'exception pour les développeurs.
            } else {
                app.UseExceptionHandler("/Home/Error"); // Gestionnaire d'exceptions pour les autres environnements.
                // HSTS (HTTP Strict Transport Security) pour une sécurité renforcée.
                app.UseHsts();
            }

            app.UseHttpsRedirection(); // Redirection HTTP vers HTTPS.
            app.UseStaticFiles(); // Serve les fichiers statiques comme le CSS, JavaScript et les images.

            app.UseRouting(); // Routage des points d'entrée.

            app.UseAuthorization(); // Application de l'authorization.

            app.UseEndpoints(endpoints => {
                endpoints.MapControllerRoute(
                    name: "default",
                    pattern: "{controller=Home}/{action=Index}/{id?}");
            });
        }
    }
}
```

### Méthode `ConfigureServices`

La méthode `ConfigureServices` est responsable de l'ajout des services au conteneur d'injection de dépendances ASP.NET Core. Dans l'exemple fourni, nous enregistrons des services tels que `IPieRepository` et `ICategoryRepository` en utilisant la méthode `AddScoped`. Cela garantit que ces services sont disponibles tout au long du cycle de vie de l'application.

### Méthode `Configure`

La méthode `Configure` définit la manière dont l'application ASP.NET Core répond aux requêtes HTTP. Elle configure les composants middleware qui traitent les requêtes et les réponses. Voici une explication des composants middleware utilisés dans l'exemple fourni :

- **Page d'Exception pour les Développeurs** : Lorsque l'application est en mode développement (`env.IsDevelopment()`), la page d'exception pour les développeurs est affichée pour fournir des informations détaillées sur les erreurs en cas d'exceptions.

- **Gestionnaire d'Exceptions** : Pour les environnements autres que le développement, un middleware gestionnaire d'exceptions est configuré pour gérer les exceptions et rediriger vers une page d'erreur.

- **HTTP Strict Transport Security (HSTS)** : HSTS est appliqué pour renforcer la communication sécurisée en indiquant au navigateur de n'interagir qu'avec le serveur via HTTPS.

- **Redirection HTTPS** : Redirige les requêtes HTTP vers HTTPS pour une communication sécurisée.

- **Fichiers Statiques** : Ce middleware sert les fichiers statiques tels que le CSS, JavaScript et les images directement depuis le répertoire spécifié.

- **Routage** : Le routage des points d'entrée est configuré pour mapper les requêtes entrantes vers le contrôleur et l'action appropriés.

- **Autorisation** : Middleware pour appliquer des politiques d'autorisation aux requêtes entrantes.

- **Points d'Entrée** : Configure le routage pour les contrôleurs et les actions MVC.

## Conclusion

Comprendre la classe `Startup` et ses méthodes est crucial pour configurer les applications ASP.NET Core. En configurant correctement les services et les middleware, les développeurs peuvent garantir la robustesse, la sécurité et le traitement efficace des requêtes dans leurs applications.

