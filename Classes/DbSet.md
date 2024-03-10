# DbSet<T>

```csharp
public abstract class DbSet<TEntity> : IQueryable<TEntity>, IEnumerable<TEntity>, IEnumerable, IQueryable, IAsyncEnumerable<TEntity>, IInfrastructure<IServiceProvider>, IListSource where TEntity : class {
        protected DbSet();
         
        public virtual LocalView<TEntity> Local { get; }
 
        public virtual EntityEntry<TEntity> Add([NotNullAttribute] TEntity entity);
         
        public virtual ValueTask<EntityEntry<TEntity>> AddAsync([NotNullAttribute] TEntity entity, CancellationToken cancellationToken = default);
         
        public virtual void AddRange([NotNullAttribute] IEnumerable<TEntity> entities);
         
        public virtual void AddRange([NotNullAttribute] params TEntity[] entities);
```


En ASP.NET Core, la classe `DbContext` fournit un lien entre votre modèle de domaine et la base de données. Elle représente une session avec la base de données, vous permettant d'interroger et de sauvegarder des instances de vos entités.

## Propriété DbSet<T>

La propriété `DbSet<T>` au sein d'une classe `DbContext` représente une collection d'entités de type `T`. Elle vous permet d'effectuer des opérations CRUD (Create, Read, Update, Delete) sur les entités de ce type dans la table de base de données associée.

### Syntaxe

```csharp
public DbSet<T> CollectionEntité { get; set; }
```

### Explication

- `public` : Spécifie l'accessibilité de la propriété. Dans ce cas, elle est accessible en dehors de la classe.
- `DbSet<T>` : Représente une collection d'entités dans le contexte d'une table de base de données. `T` désigne le type d'entité.
- `CollectionEntité` : Nom de la propriété représentant la collection d'entités.
- `{ get; set; }` : Syntaxe de propriété auto-implémentée. Elle fournit à la fois un accès en lecture et en écriture à la propriété.

### Exemple

Supposons que nous ayons une classe `Produit` représentant des produits dans une application de commerce électronique. Nous pouvons définir une propriété `DbSet<Produit>` dans notre classe `AppDbContext` :

```csharp
public class AppDbContext : DbContext
{
    public DbSet<Produit> Produits { get; set; }
}
```

Cela nous permet d'interagir avec la table `Produits` dans la base de données à l'aide d'Entity Framework Core.

### Informations supplémentaires

- **Interrogation des données** : Vous pouvez utiliser des requêtes LINQ pour récupérer des données de la base de données via la propriété `DbSet<T>`.
- **Insertion de données** : De nouvelles entités peuvent être ajoutées à la base de données en utilisant des méthodes fournies par `DbSet<T>`, telles que `Add()` ou `AddRange()`.
- **Mise à jour des données** : Les modifications apportées aux propriétés des entités sont suivies par Entity Framework Core, et vous pouvez sauvegarder ces modifications dans la base de données en utilisant la méthode `SaveChanges()`.
- **Suppression de données** : Les entités peuvent être supprimées de la base de données à l'aide de méthodes telles que `Remove()`.

## Conclusion

La propriété `DbSet<T>` dans la classe `DbContext` d'ASP.NET Core facilite l'interaction avec la base de données sous-jacente en fournissant un accès aux collections d'entités. Comprendre comment utiliser `DbSet<T>` de manière efficace est crucial pour construire des applications ASP.NET Core robustes et efficaces.