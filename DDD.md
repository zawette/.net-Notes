# DDD
## EF Core
for value objects inside an entity, EF core needs to know that its an owned type (essentialy , i think it will flatten the props of the value type inside the entity table) and in order to mark a property (value object) as an owned type (in the onModelCreating method):
```c#
modelBuilder.Entity<TEntity>().OwnsOne(t=> t.property)
//example (player is the entity ; NameFactory is the value Object)
modelBuilder.Entity<Player>().OwnsOne(p=> p.NameFactory)

```