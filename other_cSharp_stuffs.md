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

# switch

sources :

- https://docs.microsoft.com/en-us/dotnet/csharp/pattern-matching
- https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-8#switch-expressions
- https://drakelambert.dev/2019/12/C%23-8-Switch-Expressions-with-Pattern-Matching.html

## switch statement using the type pattern:

```c#
    switch (shape)
{
    case Square :
        return s.Side * s.Side;
    case Circle :
        return c.Radius * c.Radius * Math.PI;
    case Rectangle :
        return r.Height * r.Length;
    case null:
        Throw new ArgumentNullException(paramName: nameof(shape), message: "Shape must not be null");
    default:
        throw new ArgumentException(
            message: "shape is not a recognized shape",
            paramName: nameof(shape));
}
```

## when clauses in case expressions

```c#
    switch (shape)
{
    case var thing when someCondition:
        break;
    case Square s when s.Side == 0:
    case Circle c when c.Radius == 0:
        return 0;

    case Square s:
        return s.Side * s.Side;
    case Circle c:
        return c.Radius * c.Radius * Math.PI;
    default:
        throw new ArgumentException(
            message: "shape is not a recognized shape",
            paramName: nameof(shape));
}
```

## c#8 Switch expressions

```c#
public static RGBColor FromRainbow(Rainbow colorBand) =>
    colorBand switch
    {
        Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
        Rainbow.Orange => new RGBColor(0xFF, 0x7F, 0x00),
        Rainbow.Yellow => new RGBColor(0xFF, 0xFF, 0x00),
        Rainbow.Green  => new RGBColor(0x00, 0xFF, 0x00),
        Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
        Rainbow.Indigo => new RGBColor(0x4B, 0x00, 0x82),
        Rainbow.Violet => new RGBColor(0x94, 0x00, 0xD3),
        _              => throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand)),
    };

public string getExceptionString(Exception exception)
            => exception switch
            {
                DomainException ex => = ex.Message,
                ApplicationException ex =>  ex.Message,
                _ => "error"
            };

```

## Property patterns

```c#
static double GetAreaExpression(this Shape shape) => shape switch
{
    {someproperty:"someValue"} => -1,
    Rectangle { Width: 0 } => 0,
    Rectangle { Height: 0 } => 0,
    Circle { Radius: 0 } circle => circle.Radius,
    Rectangle rectangle => rectangle.Height * rectangle.Width,
    Circle circle => Math.PI * circle.Radius * circle.Radius,
    _ => throw new ArgumentException()
};
```

## Recursive Pattern Matching

```c#
var message = complexType switch
{
    { ShipmentStatus: Shipment.State.Ordered } => "Congrats on your order!",
    { Address: { State: "LA" } } => "I live there too!",
    { Address: { Zip: null } } => "You forgot to enter a zip code!",
    { ShipmentStatus: Shipment.State.Delivered, Name: (var firstName, _) } => $"Enjoy your package {firstName}!",
    null => throw new ArgumentNullException(),
    _ => "I'm not sure what I'm looking at here."
};
```

## Tuple patterns

```c#
public static string RockPaperScissors(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "rock is covered by paper. Paper wins.",
        ("rock", "scissors") => "rock breaks scissors. Rock wins.",
        ("paper", "rock") => "paper covers rock. Paper wins.",
        ("paper", "scissors") => "paper is cut by scissors. Scissors wins.",
        ("scissors", "rock") => "scissors is broken by rock. Rock wins.",
        ("scissors", "paper") => "scissors cuts paper. Scissors wins.",
        (_, _) => "tie"
    };
```
