# passing data to the api 
## attributes
+ [fromBody]
+ [fromForm]
+ [fromHeader]
+ [fromQuery]
+ [fromRoute]
+ [fromService]

example:
```c#
public IActionResult getSmtin([FromRoute] Guid smtinId)
```
## default behaviour when using [apiController] attribute 
+ [fromBody] : inferred for complex types
+ [fromRoute] : Inferred for any action parameter name matching a parameter in the route template
+ [fromQuery] : Inferred for any other action parameter
+ automatically returns a 400 BadRequest with the validation error message on the response body on validation errors

## route with an array of things
working with an array of ids for example  "api/courses/(3,4,8,9,8)" using a custom model binder

``` c# 
    public class ArrayModelBinder : IModelBinder
    {
        public Task BindModelAsync(ModelBindingContext bindingContext)
        {
            // Our binder works only on enumerable types
            if (!bindingContext.ModelMetadata.IsEnumerableType)
            {
                bindingContext.Result = ModelBindingResult.Failed();
                return Task.CompletedTask;
            }

            // Get the inputted value through the value provider
            var value = bindingContext.ValueProvider
                .GetValue(bindingContext.ModelName).ToString();

            // If that value is null or whitespace, we return null
            if (string.IsNullOrWhiteSpace(value))
            {
                bindingContext.Result = ModelBindingResult.Success(null);
                return Task.CompletedTask;
            }

            // The value isn't null or whitespace, 
            // and the type of the model is enumerable. 
            // Get the enumerable's type, and a converter 
            var elementType = bindingContext.ModelType.GetTypeInfo().GenericTypeArguments[0];
            var converter = TypeDescriptor.GetConverter(elementType);

            // Convert each item in the value list to the enumerable type
            var values = value.Split(new[] { "," }, StringSplitOptions.RemoveEmptyEntries)
                .Select(x => converter.ConvertFromString(x.Trim()))
                .ToArray();

            // Create an array of that type, and set it as the Model value 
            var typedValues = Array.CreateInstance(elementType, values.Length);
            values.CopyTo(typedValues, 0);
            bindingContext.Model = typedValues;

            // return a successful result, passing in the Model 
            bindingContext.Result = ModelBindingResult.Success(bindingContext.Model);
            return Task.CompletedTask;
        }
    }
```
src: https://app.pluralsight.com/player?course=asp-dot-net-core-3-restful-api-building&author=kevin-dockx&name=2aa0f8df-4d24-4f55-af94-35a076db05a8&clip=6&mode=live

## custom validation 
custom validation for a dto is possible by either by :
+ extending the IValidatableObject interface and implementing the Validate method 
+ class level input validation with a custom attribute (class that extends ValidationAttribute and implements IsValid)

## DI IOC
+ By calling services.AddSingleton will create the service the first time you request it and then every subsequent request is calling the same instance of the service. This means that all components are sharing the same service every time they need it. You are using the same instance always
+ By calling services.AddScoped will create the service once per request. That means whenever we send the HTTP request towards the application, a new instance of the service is created
+ By calling services.AddTransient will create the service each time the application request it. This means that if during one request towards our application, multiple components need the service, this service will be created again for every single component which needs it
