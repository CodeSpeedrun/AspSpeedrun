# TagHelper ASP.NET Core

## Introduction
Les TagHelpers sont une fonctionnalité unique dans ASP.NET Core qui permet aux développeurs de créer des composants réutilisables pour générer du HTML dynamiquement. Ils offrent une manière déclarative d'interagir avec les éléments HTML dans les vues Razor. Ce document détaillera les subtilités des TagHelpers, fournissant des explications détaillées, des extraits de code, des exemples et plus encore.

## Vue d'ensemble
Les TagHelpers dans ASP.NET Core sont des classes qui exécutent du code pour modifier ou générer des éléments HTML dans les vues Razor. Ils encapsulent des composants d'interface utilisateur réutilisables et simplifient le processus de création de pages Web dynamiques. Contrairement aux aides HTML traditionnelles ou à la syntaxe Razor, les TagHelpers ressemblent étroitement à des balises HTML, ce qui les rend plus intuitifs et lisibles.

## Anatomie d'un TagHelper
Une classe TagHelper se compose généralement des éléments suivants :

### 1. Déclaration de la classe TagHelper
```csharp
public class CustomTagHelper : TagHelper
{
    // Implémentation du TagHelper
}
```
Dans cet exemple, `CustomTagHelper` est le nom de la classe TagHelper, qui hérite de la classe de base `TagHelper`.

### 2. Propriétés du TagHelper
Les propriétés dans une classe TagHelper représentent des attributs pouvant être définis dans la balise HTML correspondante. Ces propriétés peuvent être annotées avec `[HtmlAttributeName]` pour spécifier le nom de l'attribut auquel elles se lient.

```csharp
[HtmlAttributeName("nom-attribut")]
public string NomAttribut { get; set; }
```
Ici, `NomAttribut` est une propriété qui se lie à l'attribut HTML avec le nom "nom-attribut".

### 3. Méthode Process
La méthode `Process` est l'endroit où réside la logique principale du TagHelper. Elle manipule le contenu HTML de la balise associée.

```csharp
public override void Process(TagHelperContext context, TagHelperOutput output)
{
    // Modifier ou générer la sortie HTML
}
```
La méthode `Process` prend deux paramètres : `TagHelperContext` fournit des informations sur l'élément HTML en cours de traitement, et `TagHelperOutput` représente le HTML généré en sortie par le TagHelper.

## Création d'un TagHelper personnalisé
Créons un TagHelper simple qui génère un élément HTML personnalisé avec un contenu dynamique.

```csharp
[HtmlTargetElement("element-personnalise")]
public class CustomTagHelper : TagHelper
{
    public override void Process(TagHelperContext context, TagHelperOutput output)
    {
        output.TagName = "div";
        output.Content.SetHtmlContent("<p>Ceci est un élément personnalisé généré par un TagHelper.</p>");
    }
}
```
Dans cet exemple, le TagHelper cible la balise `<element-personnalise>` et la remplace par une balise `<div>` contenant un paragraphe avec un contenu personnalisé.

## Utilisation des TagHelpers dans les vues Razor
Pour utiliser un TagHelper dans une vue Razor, ajoutez le namespace du TagHelper au fichier `_ViewImports.cshtml`.

```csharp
@addTagHelper *, VotreNamespaceDuProjet
```
Remplacez `VotreNamespaceDuProjet` par le namespace contenant vos TagHelpers.

Maintenant, vous pouvez utiliser le TagHelper dans n'importe quelle vue Razor.

```html
<element-personnalise></element-personnalise>
```
Cela sera rendu comme suit :

```html
<div>
    <p>Ceci est un élément personnalisé généré par un TagHelper.</p>
</div>
```
Ex 2
```csharp
public class EmailTagHelper : TagHelper
    {
        public string Address { get; set; }
        public string Content { get; set; }

        public override void Process(TagHelperContext context, TagHelperOutput output)
        {
            output.TagName = "a";

            output.Attributes.SetAttribute("href", "mailto:" + Address);
            output.Content.SetContent(Content);
        }
    }
```

## Conclusion
Les TagHelpers sont un outil puissant dans ASP.NET Core pour créer des composants réutilisables et dynamiques dans les vues Razor. En comprenant leur anatomie et leur utilisation, les développeurs peuvent améliorer leur productivité et créer des applications Web plus maintenables. Expérimentez avec différents scénarios et explorez les fonctionnalités avancées pour tirer pleinement parti des TagHelpers dans vos projets.