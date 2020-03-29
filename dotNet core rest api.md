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
