# .net mvc5
## .net mvc5 dependency injection
using autofac.mv5 :

Create a container Config class that implements a method for registering the container inside App_Start:
``` c#
 public class ContainerConfig
    {
        internal static void RegisterContainer()
        {
            var builder = new ContainerBuilder();
            builder.RegisterControllers(typeof(MvcApplication).Assembly); //letting autofac know what assembly contains the controller (MvcApplication exists in Global.asax.cs)
            builder.RegisterType<InMemoryRestaurantData>()
                   .As<IRestaurantData>()
                   .SingleInstance();

            var container = builder.Build();
            DependencyResolver.SetResolver(new AutofacDependencyResolver(container));
        }
    }
```
call RegisterContainer() from within Global.asax.cs:

``` c#
 public class MvcApplication : System.Web.HttpApplication
    {
        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
            ContainerConfig.RegisterContainer();
        }
    }
```

src: https://app.pluralsight.com/course-player?clipId=f1f8c7d9-70fe-4b48-8bda-72f24215aea4

## validating models 
 using ModelState: 

``` c#
[HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create(Restaurant restaurant)
        {
            if(String.IsNullOrEmpty(restaurant.Name)){
                ModelState.AddModelError(nameof(restaurant.Name),"the name is required"); // could've used Data Annotation in the restaurant Model, this is just showing that its possible
            }
            if (ModelState.IsValid)
            {
                db.Add(restaurant);
                return RedirectToAction("Details", new { id = restaurant.Id });
            }
            return View();
        }
```
## ViewData, ViewBag & TempData in ASP dot NET MVC 5

https://www.c-sharpcorner.com/article/viewdata-viewbag-tempdata-in-asp-net-mvc-5/