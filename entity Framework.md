# Entity Framework 6
## conventions
+ https://www.entityframeworktutorial.net/code-first/code-first-conventions.aspx 
+ these conventions could be overriden by ( Data annotations attributes or Fluent API) as explained here in this link
 https://www.entityframeworktutorial.net/code-first/configure-classes-in-code-first.aspx

## Database Initialization Strategies in EF 6 Code-First
+ the db initializer could be set either from within the constructor of the  class that extend DbContext or from the configuration file (app.config)
+ there are 3 types of db initializers and a custom db initializers could be made by extending one of the three initial db initializers:
    + CreateDatabaseIfNotExists (the defautlt initializer)
    + DropCreateDatabaseIfModelChanges
    + DropCreateDatabaseAlways
+  https://www.entityframeworktutorial.net/code-first/database-initialization-strategy-in-code-first.aspx

## Inheritance Strategies in EF6 ( READ LATER )
+ https://www.entityframeworktutorial.net/code-first/inheritance-strategy-in-code-first.aspx

## Loading related Data in EF6
+ eager Loading : using include()  // this brings all the data from the database in one call
``` c# 
// example from the course https://app.pluralsight.com/course-player?clipId=0746c14b-88a1-4d6e-be4a-286c28a59523
using (var context = new NinjaContext())
{
    var ninja = context.Ninjas.Include(n=>n.EquipementOwned).FirstOrDefault();
}

``` 
+ explicit Loading: loading related data just for a specific item / number of items 
``` c# 
// example from the course https://app.pluralsight.com/course-player?clipId=0746c14b-88a1-4d6e-be4a-286c28a59523
using (var context = new NinjaContext())
{
    var ninja = context.Ninjas.FirstOrDefault(); // loading one ninja in this case
    context.Entry(ninja).Collection(n=>n.EquipementOwned).Load(); // getting the related collection (EquipementOwned)
}

``` 
+ Lazy Loading: By Marking the related property as virtual, then just mentionting the property will trigger EF6 to go retrive it  https://app.pluralsight.com/course-player?clipId=0746c14b-88a1-4d6e-be4a-286c28a59523

+ Loading related Data using projection queries: 
``` c# 
using (var context = new NinjaContext())
{
    var ninja = context.Ninjas.Select(n => new {n.name, n.dateOfBirth, m.EquipementOwned}).toList();
}

``` 