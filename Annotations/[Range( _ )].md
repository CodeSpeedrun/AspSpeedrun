
### `[Range(minimum, maximum)]`

Cet attribut permet de spécifier une plage de valeurs autorisées pour une propriété numérique.

Exemple:
```csharp
public class Produit
{
    [Range(1, 100)]
    public int Quantite { get; set; }
}
```

Dans cet exemple, la propriété `Quantite` doit être comprise entre 1 et 100.
