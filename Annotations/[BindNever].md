
### Attribut `[BindNever]`

L'attribut `[BindNever]` est utilisé pour exclure une propriété de la liaison de modèle. Cela signifie que la propriété ne sera pas remplie avec les valeurs des demandes HTTP entrantes. Cela peut être utile lorsque vous avez des propriétés sensibles ou calculées qui ne doivent pas être directement définies par l'entrée utilisateur.

Exemple:
```csharp
public class Commande
{
    [BindNever]
    public DateTime DateCommande { get; set; }
}
```

Dans cet exemple, la propriété `DateCommande` ne sera pas remplie avec les valeurs des demandes entrantes, garantissant que la date de commande est déterminée par le serveur plutôt que par l'entrée utilisateur.
