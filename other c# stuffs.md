# Dynamic
## ExpandoObject
a dynamic dictionnary , behaves like a javascript object

```c#
dynamic person= new ExpandoObject();
person.profession="baker";  // person.add("profession","baker")
person.print= (Action) (()=>{
    foreach ( var item in person){
        Console.WriteLine($"{item.key }{item.value}");
    }
})
```