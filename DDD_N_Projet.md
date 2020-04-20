# DDD & SchoolClassReservationProject
## DD
### EF Core
for value objects inside an entity, EF core needs to know that its an owned type (essentialy , i think it will flatten the props of the value type inside the entity table) and in order to mark a property (value object) as an owned type (in the onModelCreating method):
```c#
modelBuilder.Entity<TEntity>().OwnsOne(t=> t.property)
//example (player is the entity ; NameFactory is the value Object)
modelBuilder.Entity<Player>().OwnsOne(p=> p.NameFactory)

```
# SchoolClassReservation
link to the repo : [SchoolClassReservation](https://github.com/zawette/SchoolClassReservation)
## notes
+ the API project depends on the "Application" and "Persistance" layers
+ the Application layer contains interfaces, Dtos , command/queries , exceptions and all application logic , it only depends on the domain layer
    + it defines interfaces that are implemented by outside layers
+ cqrs: seperates reads(queries) from writes(commands)
    + can maximize the performance , scalability, and simplicity
    + easy to mantain (changes only affect one command or query)
+ the project is not complex enough to justify using the cqrs pattern, i only implemented it for learning purposes 
+ Domain layer contains entities and value types and logic specific to the domain 
+ the presentation layer can be changed with minimal effort
## technologies
+ ASP.NET Core 3
+ Entity Framework Core 
+ mediatR
+ Identity

