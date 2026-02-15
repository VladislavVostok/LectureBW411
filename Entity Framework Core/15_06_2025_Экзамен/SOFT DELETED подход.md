Soft delete (мягкое удаление) в Entity Framework — это подход, при котором записи помечаются неактивными (например, через поле `IsDeleted` или `DeletedDate`), а не удаляются из БД физически. Это позволяет сохранять историю, восстанавливать данные и поддерживать целостность ссылок. Реализуется через переопределение `SaveChanges` или использование фильтров запросов (Global Query Filters).


```cs
public interface ISoftDeletable
{
    bool IsDeleted { get; set; }
}

public class Book : ISoftDeletable
{
    public int Id { get; set; }
    public string Title { get; set; }
    public bool IsDeleted { get; set; }
}

```


```cs
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    foreach (var entityType in modelBuilder.Model.GetEntityTypes())
    {
        if (typeof(ISoftDeletable).IsAssignableFrom(entityType.ClrType))
        {
            entityType.AddAnnotation("IsSoftDelete", true);
            modelBuilder.Entity(entityType.ClrType)
                .HasQueryFilter(e => !EF.Property<bool>(e, "IsDeleted"));
        }
    }
}
```

```cs
public override int SaveChanges()
{
    HandleSoftDeletes();
    return base.SaveChanges();
}

public override Task<int> SaveChangesAsync(CancellationToken cancellationToken = default)
{
    HandleSoftDeletes();
    return base.SaveChangesAsync(cancellationToken);
}

private void HandleSoftDeletes()
{
    var entries = ChangeTracker.Entries<ISoftDeletable>()
        .Where(e => e.State == EntityState.Deleted);

    foreach (var entry in entries)
    {
        entry.State = EntityState.Modified; // Change state from Deleted to Modified
        entry.Entity.IsDeleted = true; // Set the flag
    }
}

```