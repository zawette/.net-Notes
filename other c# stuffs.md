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
extending DynamicObject( a class that extends IDynamicMetaObjectProvider,enables you to define for example what happens when you try to get or set an object property, call a method or perform mathematical operation such as addition and multiplication)
 
## some tests
```c#
    class DynamicTests
    {
        // without dynamic , we would have to make methods for all possible value types (int , float ,decimal etc)
        // using dynamic however is more performance heavy than using specific types
        public static T AddAnyNumericDynamic<T>(T a, T b)
        {
            //casting dynamic because otherwise it wouldn't work (T doesn't know what to do with the plus Operand)
            T res = (dynamic)a + b;
            return res;
        }

        public static void DeserializeJsonObjectDynamic(string jsonObject)
        {
            dynamic myObject = JsonConvert.DeserializeObject(jsonObject);
            Console.WriteLine($"firstName: {myObject.firstName} lastName:{myObject.lastName}");
        }

        public static void DeserializeJsonObjectReflection<T>(string jsonObject)
        {
            T myObject = JsonConvert.DeserializeObject<T>(jsonObject);
            Type type = myObject.GetType();
            var properties = type.GetProperties();
            foreach (var property in properties)
            {
                Console.WriteLine($"{property.Name}: {property.GetValue(myObject)} ");
            }
        }

    }
    ```
