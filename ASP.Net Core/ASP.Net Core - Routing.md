# ASP.Net Core - Routing

The Basics
----

- Routing is a term used in ASP.NET Core for a _**System**_  which takes URLs and maps them to controller actions, files, or other items. 

- The said system involves the use of _**classes**_ called _**route handlers**_, which actually do the mapping. 

- In actual reality, we won't be creating routes at such a low level (e.g. creating _**route handlers**_), rather we will be defining routes and telling ASP.NET Core where those routes map to.

- There are two main ways to define routes:

- Convention-based routing, defined in **Startup.cs**, creates routes based on a series of conventions

- Attribute routing, defined in **Controller.cs**, creates routes based on [**Route(<route-template-we-provide>)**] attributes and be placed on **Controller's** Actions.

- **Route Template** looks like: [Route("Home/Contact")] or [HttpGet("/Products")]

- The two **routing systems** can co-exist in the same system.

Convention-Based Routing
----

- With convention-based routing, in the Startup.cs' Configure(), we define a series of route conventions that are meant to represent all the possible routes in the system.

- Example:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    ...
    app.UseMvc(routes =>
    {
        routes.MapRoute(template: "{controller}/{action}");
    });
}
```

- Explanations - the above Convention-Based route expects, and will map, the following URLs:

    + http://abc.com/home/index
    + http://abc.com/blogs/all
    + http://abc.com/recipes/index


Route Constraints
----

- What about these URLs?

```iecst
    http://abc.com.com/blogpost/1/5
    http://abc.com.com/home/index/17
```

- One way:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    ...
    app.UseMvc(routes =>
    {
        routes.MapRoute(template: "{controller}/{action}/{id}");
    });
}
```

- But, it also match the following:

```iecst
    http://abc.com/home/index/all
    http://abc.com/recipes/index/first
    http://abc.com/samples/costco/many
```

- Which probably isn't what we want---the "all", "first", and "many" will be mapped to an Action's _id_ parameter. 

- What if we want the _id_ only take an integer?

- We would introduce a _**route constraint**:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    ...
    app.UseMvc(routes =>
    {
        routes.MapRoute(
            template: "{controller}/{action}/{id:int}");
    });
}
```

- The {id:int} specifies that whatever is in this part of the URL must be an integer, otherwise the URL does not map to the route.

- Possible constraints includes:

```iecst
    :int
    :bool
    :datetime
    :decimal
    :guid
    :length(min,max)
    :alpha
    :range(min,max)
```

- [ASP.Net: Route Constraint Reference](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing?view=aspnetcore-3.1#route-constraint-reference)

- Optional Parameters

    + **?** can be added to signal optional parameter in a route.

```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
    ...
        app.UseMvc(routes =>
        {
            routes.MapRoute(template: "{controller}/{action}/{id:int?}");
        });
    }
```

- Note: we can only have one optional parameter per route and it must be the last parameter

Default Values
----

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    ...
    app.UseMvc(routes =>
    {
        routes.MapRoute(
            template: "{controller=Home}/{action=Index}/{id:int?}");
    });
}
```

- So, http://abc.com/ would be mapped to http://abc.com/Home/Index/4

- Using **defaults** property

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    ...
    app.UseMvc(routes =>
    {
        routes.MapRoute(
            template: "{controller}/{action}/{id:int?}"),
            defaults: new { controller = "Home", action = "Index" }
    });
}
```

Named Routes
----

- Can name a route uniquely, enabling to have a multiple routes with similar parameters but that can be used differently.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    ...
    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller}/{action}/{id:int?}"),
            defaults: new { controller = "Home", action = "Index" }
    });
}
```

- Example:

```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        ...
        app.UseMvc(routes =>
        {
            routes.MapRoute(
                name: "blogPosts",
                template: "convention/{action}/{blogID:int}/{postID:int}",
                defaults: new { controller = "Convention", action = "BlogPost" }
                );

            routes.MapRoute(
                name: "allBlogs",
                template: "convention/{action}/{blogID:int?}",
                defaults: new { controller = "Convention", action = "AllBlogsAndIndex" }
                );

            routes.MapRoute(
                name: "default",
                template: "{controller=Home}/{action=Index}/{id:int?}");
        });
    }
```


Attribute Routing
----

- Attribute routing provides a better relationship between actions and routes and more fine-grained control over routes.

```csharp
public class HomeController : Controller
{
    [HttpGet("home/index")] // We generally want to use the more specific Http[Verb] attributes over the generic Route attribute
    public IActionResult Index() { return View(); }
}
```

Route Tokens
----

    1) [controler]
    2) [area]
    3) [action]


Multiple Routes
----

- We can use Attribute Routing to map multiple routes to the same Action method or controller.

```csharp
    public class UsersController : Controller
    {
        [HttpGet("~/")] // matches http://abc.com/ (aka the application's default action)
        [HttpGet("")] // matches http://abc.com/users (aka the controller's default action)
        [HttpGet("index")] // matches http://abc.com/users/index
        public IActionResult Index() { return View(); }
    }
```

Mixed Routing
----

- Can mix Convention-based routing and Attribute Routing on different Action

- But, can not be doing mixed routing on the same Action.





