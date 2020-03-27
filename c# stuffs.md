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