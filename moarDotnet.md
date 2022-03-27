## extension method for getting options
```c#
public static T GetOptions<T>(this IServiceCollection services, string sectionName) where T : new()
        {
            using var serviceProvider = services.BuildServiceProvider();
            var configuration = serviceProvider.GetRequiredService<IConfiguration>();
            var section = configuration.GetSection(sectionName);
            var options = new T();
            section.Bind(options);
            
            return options;
        }
```
or

```c#
public static T GetOptions<T>(this IServiceCollection services, string sectionName, IConfiguration config) where T : new()
        {
            var options = new T();
            config.GetSection(sectionName).Bind(options);            
            return options;
        }
```
