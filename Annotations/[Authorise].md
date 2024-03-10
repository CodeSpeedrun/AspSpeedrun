# Autorisation ASP.NET Core avec `[Authorize]`

En ASP.NET Core, l'autorisation joue un rôle crucial dans le contrôle de l'accès aux ressources au sein de votre application. Une manière d'implémenter l'autorisation est d'utiliser l'attribut `[Authorize]`. Cet attribut peut être appliqué à différents niveaux de votre application, tels que les contrôleurs, les méthodes d'action ou même les pages Razor individuelles.

## Autorisation au Niveau du Contrôleur

Considérons le scénario suivant où nous avons un `OrderController` dans notre application ASP.NET Core. Nous voulons restreindre l'accès à ce contrôleur uniquement aux utilisateurs authentifiés. Nous pouvons y parvenir en appliquant l'attribut `[Authorize]` à la classe du contrôleur.

```csharp
[Authorize]
public class OrderController : Controller
{
    // Actions et méthodes du contrôleur
}
```

### Explication:

- L'attribut `[Authorize]` est un filtre qui vérifie si l'utilisateur actuel est authentifié avant de lui permettre l'accès au contrôleur et à ses actions.
- Si l'utilisateur n'est pas authentifié, il sera redirigé vers la page de connexion ou se verra refuser l'accès, en fonction de la configuration de l'authentification.
- Cet attribut fait partie de l'infrastructure d'autorisation ASP.NET Core et est utilisé pour mettre en œuvre une autorisation basée sur les rôles ou les politiques.

### Plus de Détails:

- **Authentification:** Avant de comprendre l'autorisation, il est essentiel de comprendre l'authentification. L'authentification vérifie l'identité d'un utilisateur.
- **Autorisation:** Une fois authentifié, l'autorisation détermine si l'utilisateur authentifié dispose des autorisations nécessaires pour accéder à une ressource.

### Exemple:

Supposons un scénario où seuls les utilisateurs enregistrés sont autorisés à consulter les commandes. En appliquant `[Authorize]` au niveau du contrôleur, nous nous assurons que seuls les utilisateurs authentifiés peuvent accéder à n'importe quelle action dans `OrderController`.

```csharp
[Authorize]
public class OrderController : Controller
{
    // Actions pour gérer les commandes
}
```

Dans cet exemple, toute requête aux points de terminaison de `OrderController` passera d'abord par le middleware d'authentification. Si l'utilisateur est authentifié, la requête passe au contrôleur. Sinon, l'utilisateur est redirigé vers la page de connexion ou reçoit une réponse non autorisée.

## Conclusion

L'attribut `[Authorize]` dans ASP.NET Core fournit un moyen simple mais puissant d'implémenter l'authentification au niveau du contrôleur. En l'appliquant de manière stratégique, vous pouvez contrôler l'accès aux ressources de votre application en fonction du statut d'authentification de l'utilisateur, garantissant la sécurité et l'intégrité de votre application.