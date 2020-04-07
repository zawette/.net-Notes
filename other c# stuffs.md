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
## creating custom Dynamic class 
by extending DynamicObject( a class that extends IDynamicMetaObjectProvider,enables you to define for example what happens when you try to get or set an object property, call a method or perform mathematical operation such as addition and multiplication)
 