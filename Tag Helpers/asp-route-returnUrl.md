# ASP.NET Core: Comprendre `asp-route-returnUrl`

En ASP.NET Core, `asp-route-returnUrl` est une fonctionnalité utile qui permet de passer des données via les routes dans une application web. Cette fonctionnalité est couramment utilisée dans les scénarios où vous devez rediriger les utilisateurs vers une page spécifique après une action particulière, telle que la connexion ou la soumission d'un formulaire.

## Aperçu

Lorsque vous travaillez avec ASP.NET Core, vous rencontrez souvent des situations où vous devez rediriger les utilisateurs vers une URL spécifique après avoir effectué une action. L'attribut `asp-route-returnUrl` simplifie ce processus en vous permettant d'inclure l'URL souhaitée dans la balise de formulaire. Cela est particulièrement utile dans les scénarios où vous souhaitez maintenir le contexte de l'action de l'utilisateur, comme les rediriger vers la page sur laquelle ils se trouvaient précédemment.

## Implémentation

Pour utiliser `asp-route-returnUrl`, vous devez l'inclure dans la balise de formulaire de votre balisage HTML. Voici comment vous pouvez le mettre en œuvre :

```html
<form asp-route-returnUrl="@Model.RedirectUrl" method="post">
    <!-- Champs de formulaire et bouton de soumission -->
</form>
```

Dans cet exemple :
- `asp-route-returnUrl` est un attribut HTML utilisé pour spécifier le paramètre de route `returnUrl`.
- `@Model.RedirectUrl` représente la valeur de la propriété `RedirectUrl` du modèle. Cette valeur sera générée dynamiquement et insérée dans le balisage HTML.

## Exemple

Considérons un scénario où vous avez un formulaire de connexion dans votre application ASP.NET Core. Après une authentification réussie, vous voulez rediriger l'utilisateur vers la page à laquelle il essayait initialement d'accéder. Voici comment vous pouvez y parvenir :

### Action du contrôleur

```csharp
public IActionResult Connexion(string returnUrl = null)
{
    var model = new LoginViewModel
    {
        ReturnUrl = returnUrl
    };
    return View(model);
}

[HttpPost]
public IActionResult Connexion(LoginViewModel model)
{
    // Logique d'authentification

    if (ModelState.IsValid)
    {
        // Rediriger l'utilisateur vers returnUrl s'il est fourni
        if (!string.IsNullOrEmpty(model.ReturnUrl) && Url.IsLocalUrl(model.ReturnUrl))
        {
            return Redirect(model.ReturnUrl);
        }
        else
        {
            // Rediriger vers la page par défaut
            return RedirectToAction("Index", "Home");
        }
    }

    // Si l'authentification échoue, renvoyer la vue de connexion avec des erreurs
    return View(model);
}
```

### Vue (Login.cshtml)

```html
@model LoginViewModel

<form asp-controller="Compte" asp-action="Connexion" asp-route-returnUrl="@Model.ReturnUrl" method="post">
    <!-- Champs de formulaire pour le nom d'utilisateur et le mot de passe -->
    <button type="submit">Connexion</button>
</form>
```

Dans cet exemple :
- La méthode d'action `Connexion` dans le contrôleur accepte un paramètre `returnUrl` facultatif, qui représente l'URL vers laquelle l'utilisateur est redirigé après une connexion réussie.
- Le formulaire de connexion dans la vue inclut l'attribut `asp-route-returnUrl`, qui capture la valeur `ReturnUrl` du modèle et l'envoie avec la soumission du formulaire.
- Lors de l'authentification réussie, le contrôleur vérifie si un `returnUrl` est fourni. Si tel est le cas, il redirige l'utilisateur vers cette URL ; sinon, il redirige vers la page par défaut.

## Conclusion

L'attribut `asp-route-returnUrl` dans ASP.NET Core simplifie le processus de redirection des utilisateurs vers des URL spécifiques après avoir effectué des actions telles que la connexion ou la soumission de formulaires. En comprenant son implémentation et son utilisation, vous pouvez améliorer l'expérience utilisateur et maintenir le contexte des interactions utilisateur dans vos applications web.