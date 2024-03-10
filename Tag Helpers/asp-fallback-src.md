# ASP.NET Core: Compréhension de `asp-fallback-src`

L'attribut `asp-fallback-src` dans ASP.NET Core est un aspect crucial lorsqu'il s'agit de scripts côté client, en particulier dans les scénarios où une ressource principale pourrait échouer à se charger. Il fournit un mécanisme de secours pour garantir une dégradation gracieuse de la fonctionnalité ou de la présentation si la ressource principale n'est pas disponible.

## Vue d'ensemble

En développement web, il est essentiel de prendre en compte les scénarios où la ressource principale, telle qu'un fichier JavaScript ou CSS, pourrait ne pas se charger en raison de diverses raisons telles que des problèmes de réseau ou de serveur. `asp-fallback-src` permet aux développeurs de définir une ressource alternative à charger dans de tels cas.

## Utilisation

### Syntaxe

```html
<script src="script-principal.js" asp-fallback-src="script-de-secours.js" asp-fallback-test="window.scriptPrincipalCharge"></script>
```

- **`src`**: Spécifie la source du script principal.
- **`asp-fallback-src`**: Spécifie la source du script de secours.
- **`asp-fallback-test`**: Définit l'expression JavaScript pour évaluer si le script principal a été chargé avec succès.

### Explication

- Si le navigateur échoue à charger le script principal (`script-principal.js` dans l'exemple), il tentera de charger le script de secours (`script-de-secours.js`) spécifié dans l'attribut `asp-fallback-src`.
- L'attribut `asp-fallback-test` permet de définir une expression JavaScript (`window.scriptPrincipalCharge` dans l'exemple) pour vérifier si le script principal a été chargé avec succès. Si cette expression évalue à `false`, le script de secours sera chargé.

### Exemple

Considérons le scénario suivant :

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js" asp-fallback-src="~/lib/jquery/dist/jquery.min.js" asp-fallback-test="window.jQuery">
</script>
```

- Ici, si le navigateur échoue à charger jQuery depuis le CDN (`https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js`), il reviendra à la copie locale spécifiée dans `~/lib/jquery/dist/jquery.min.js`.
- L'attribut `asp-fallback-test` vérifie si `window.jQuery` est défini, ce qui indique le chargement réussi du script principal.

## Détails de l'implémentation

### Gestion des Secours dans ASP.NET Core

Dans une application ASP.NET Core, vous pouvez gérer les secours en utilisant le middleware `UseStaticFiles` dans la méthode `Configure` de `Startup.cs`. Par exemple :

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles(new StaticFileOptions
    {
        OnPrepareResponse = ctx =>
        {
            // Vérifier si la demande concerne un fichier de script
            if (ctx.File.Name.EndsWith(".js"))
            {
                // Ajouter le script de secours si le script principal n'est pas trouvé
                ctx.Context.Response.Headers.Add("asp-fallback-test", "window.scriptPrincipalCharge");
                ctx.Context.Response.Headers.Add("asp-fallback-src", "~/js/script-de-secours.js");
            }
        }
    });
}
```

Dans ce code :
- L'événement `OnPrepareResponse` de `StaticFileOptions` est utilisé pour intercepter les demandes de fichiers de script.
- Si le fichier de script demandé n'est pas trouvé, les en-têtes `asp-fallback-test` et `asp-fallback-src` appropriés sont ajoutés à la réponse.

## Conclusion

Comprendre et utiliser `asp-fallback-src` dans ASP.NET Core est essentiel pour un scripting côté client robuste, assurant que votre application web se comporte de manière gracieuse même dans des conditions défavorables. En fournissant des ressources de secours, vous améliorez l'expérience utilisateur et atténuez les problèmes potentiels liés aux échecs de chargement des ressources.