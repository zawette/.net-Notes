# C# stuffs
+ ## strings
    + Strings in C sharp are reference type that acts like a value type
    ``` c#
     var string1= "something"; var string2=string1; string2="sumtin";         // string1="something" et string2="sumtin"
    ```
+ ## lists
    + avoid using ArrayList , it dosen't use generics, it simply casts an object (if its a list of a value type it would box it => waste of performance), use "System.Collections.Generic" collections instead


# linq's "where" method reimplementation and some other conclusions 
+ ## no lazy loading

``` c#
public static IEnumerable<T> where(this IEnumerable<T> source, Func<T,bool>) predicate{
    List<T> outp=new List<T>
    foreach(T item of source){
        if(predicate(item)){
            out.Add(item)
        }
    return outp
    }        
}
```
+ ## with lazy loading

``` c#
> public static IEnumerable<T> where(this IEnumerable<T> source, Func<T,bool>) predicate{
    foreach(T item of source){
        if(predicate(item)){
            yield return item
        }
    }        
}
```

## conclusions
> avoid  deffered execution (lazy loading) if you have to use later on your code operators that execute immediately like "count" since that would mean looping through the whole set multiple times 

# IEnumerable vs IQueryable and  Expression<Func<T, bool>> vs Func<T, bool>

I didn't need to understand the difference until I walked into a really annoying 'bug' trying to use LINQ-to-SQL generically:

public IEnumerable<T> Get(Func<T, bool> conditionLambda){
  using(var db = new DbContext()){
    return db.Set<T>.Where(conditionLambda);
  }
}

This worked great until I started getting OutofMemoryExceptions on larger datasets. Setting breakpoints inside the lambda made me realize that it was iterating through each row in my table one-by-one looking for matches to my lambda condition. This stumped me for a while, because why the heck is it treating my data table as a giant IEnumerable instead of doing LINQ-to-SQL like it's supposed to? It was also doing the exact same thing in my LINQ-to-MongoDb counterpart.

The fix was simply to turn Func<T, bool> into Expression<Func<T, bool>>, so I googled why it needs an Expression instead of Func, ending up here.

An expression simply turns a delegate into a data about itself. So a => a + 1 becomes something like "On the left side there's an int a. On the right side you add 1 to it." That's it. You can go home now. It's obviously more structured than that, but that's essentially all an expression tree really is--nothing to wrap your head around.

Understanding that, it becomes clear why LINQ-to-SQL needs an Expression, and a Func isn't adequate. Func doesn't carry with it a way to get into itself, to see the nitty-gritty of how to translate it into a SQL/MongoDb/other query. You can't see whether it's doing addition or multiplication or subtraction. All you can do is run it. Expression, on the other hand, allows you to look inside the delegate and see everything it wants to do. This empowers you to translate the delegate into whatever you want, like a SQL query. Func didn't work because my DbContext was blind to the contents of the lambda expression. Because of this, it couldn't turn the lambda expression into SQL; however, it did the next best thing and iterated that conditional through each row in my table.

Edit: expounding on my last sentence at John Peter's request:

IQueryable extends IEnumerable, so IEnumerable's methods like Where() obtain overloads that accept Expression. When you pass an Expression to that, you keep an IQueryable as a result, but when you pass a Func, you're falling back on the base IEnumerable and you'll get an IEnumerable as a result. In other words, without noticing you've turned your dataset into a list to be iterated as opposed to something to query. It's hard to notice a difference until you really look under the hood at the 

src :  Chad Hedgcock's answer https://stackoverflow.com/questions/793571/why-would-you-use-expressionfunct-rather-than-funct