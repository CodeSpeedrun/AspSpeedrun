# ASP.NET Core : Redirection vers une autre action

En ASP.NET Core, rediriger d'une action vers une autre est une pratique courante, notamment après avoir effectué certaines opérations telles que la soumission de formulaire ou la finalisation d'un processus de paiement. La méthode `RedirectToAction` est utilisée pour accomplir cette tâche de manière transparente.

## Utilisation de la méthode RedirectToAction

```csharp
return RedirectToAction("ValidationTerminée");
```

### Explication :

La méthode `RedirectToAction` est invoquée à l'intérieur d'une action de contrôleur pour rediriger la requête vers une autre action du même contrôleur ou d'un autre contrôleur. Ici, `"ValidationTerminée"` est le nom de l'action vers laquelle la requête sera redirigée.

#### Paramètres :

- **actionName** : Ce paramètre spécifie le nom de la méthode d'action vers laquelle la requête sera redirigée.
- **controllerName** : Facultativement, vous pouvez spécifier le nom du contrôleur vers lequel la requête sera redirigée. S'il n'est pas spécifié, le contrôleur actuel est supposé.
- **routeValues** : Ce paramètre vous permet de passer des valeurs de route supplémentaires à la méthode d'action à laquelle vous êtes redirigé.

### Informations supplémentaires :

- **Redirection vers un autre contrôleur** : Si l'action à laquelle vous souhaitez rediriger se trouve dans un contrôleur différent, vous pouvez spécifier également le nom du contrôleur. Par exemple :
  ```csharp
  return RedirectToAction("Index", "Accueil");
  ```
  Cela redirige vers l'action `Index` du contrôleur `Accueil`.

- **Passage de valeurs de route** : Vous pouvez passer des valeurs de route supplémentaires à la méthode d'action à laquelle vous êtes redirigé. Par exemple :
  ```csharp
  return RedirectToAction("Détails", "Produit", new { id = identifiantProduit });
  ```
  Cela redirige vers l'action `Détails` du contrôleur `Produit`, en passant une valeur de route `id` avec la valeur de `identifiantProduit`.

- **Utilisation de TempData pour la redirection** : Si vous avez besoin de conserver des données lors d'une redirection, vous pouvez utiliser `TempData`. TempData est un objet dictionnaire qui persiste pour une seule requête. Par exemple :
  ```csharp
  TempData["Message"] = "Validation terminée avec succès !";
  return RedirectToAction("ValidationTerminée");
  ```
  Dans l'action `ValidationTerminée`, vous pouvez récupérer le message depuis TempData et l'afficher à l'utilisateur.

- **Redirection avec un code d'état HTTP** : Par défaut, `RedirectToAction` émet une réponse 302 Found. Vous pouvez également spécifier explicitement le code d'état HTTP :
  ```csharp
  return RedirectToActionPermanent("ValidationTerminée");
  ```
  Cela émettra une réponse 301 Moved Permanently, indiquant que la ressource demandée a été déplacée de façon permanente vers un nouvel emplacement.

```csharp
 public RedirectToActionResult AddToShoppingCart(int pieId) {
    var selectedPie = _pieRepository.AllPies.FirstOrDefault(p => p.PieId == pieId);

    if (selectedPie != null) {
        _shoppingCart.AddToCart(selectedPie, 1);
    }
    return RedirectToAction("Index");
}
```

### Conclusion :

La méthode `RedirectToAction` offre un moyen pratique de rediriger les utilisateurs vers différentes actions au sein d'une application ASP.NET Core. Que ce soit pour rediriger à l'intérieur du même contrôleur ou vers un contrôleur différent, avec ou sans valeurs de route supplémentaires, cette méthode offre une flexibilité et une facilité d'utilisation dans la gestion du flux de l'application.
 