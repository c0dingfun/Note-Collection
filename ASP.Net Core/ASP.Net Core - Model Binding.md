Model Binding
----

[Ref](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/model-binding?view=aspnetcore-3.1 "Model Binding")

What is Model Binding?
----

- The process by which ASP.Net Core takes an HTTP request and "binds" pieces of that request, as well as other data sources, to inputs (e.g parameters) on a Controller Action.

- Example:

    + http://www.abc.com/blog/posts/14

- Using the default route: {controller}/{action}/{id?}

- The BlogController's Post() action is called with a parameter name **id** and **value** 14 passed to it.


Data Sources
----

- Three (3) primary data sources are used to map the HTTP requests to actions, in following order:

    1) Form values: Values in the **FORM** in HTTP POST requests, with a parameter called _**id**_.

    2) Route values: Values provided by the Routing system, using the route such as {controller}/{action}/{id?}

    3) Query string: Values found in the URL's query string (eg. after the ? character)


Binding Complex Types
----

- To data bind to something more complex types rather than simple types, eg: int, string, etc. what to do?

- ASP.Net Core uses both reflection and recursion to bind complex types. 

- Example:

```csharp
public class User 
{
    public string FirstName { get; set; }
}
```

- with http://www.abc.com/users/create?firstName=Hariya

- The model binding system will use reflection to discover that the _**User**_ class has a FirstName property, and bind to it the **firstName** parameter's value "Hariya" found in the query string.

- "Data Binding" is powerful, but in a more complex situation, though, always use attributes on the complex type's property.


Custom Model Binding with Attributes
----

- In addition to the default binding methodology, ASP.Net Core offers a more customized way to accomplish model binding by the use of attributes. Here's a few of attributes offered by ASP.Net Core:

    + [BindRequired]: If binding cannot happen, this attribute adds a ModelState error.

    + [BindNever]: Tells the model binder to ignore this parameter.

    + [FromHeader]: Forces binding from the HTTP request header.

    + [FromQuery]: Forces binding from the URL's query string.

    + [FromServices]: Binds the parameter from services provided by dependency injection.

    + [FromRoute]: Forces binding from values provided by Routing.

    + [FromForm]: Forces binding from values in the FORM.

    + [FromBody]: Forces binding from values in the body of the HTTP request.

- You can also perform completely custom binding using your own binder. [Ref](https://docs.microsoft.com/en-us/aspnet/core/mvc/advanced/custom-model-binding)

Default Model Binding Examples
----

```csharp
public class User
{
    public int ID { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public DateTime DateOfBirth { get; set; }
    public string Controller { get; set; }
}

public class DefaultController : Controller
{
    [HttpGet] //Bound from Query String
    public IActionResult Index(string message) 
    {
        ViewData["message"] = message;
        return View();
    }

    [HttpGet]//Bound from Routing System
    public IActionResult New(string controller) 
    {
        User user = new User();
        user.Controller = controller;
        return View(user);
    }

    [HttpPost] //Bound from FORM if and only if has "controller" property
    public IActionResult New(User user) 
    {
        var message = user.FirstName + " " + user.LastName + ", Date of Birth: " + user.DateOfBirth.ToString("yyyy-MM-dd") + ", ID: " + user.ID + ", Controller: " + user.Controller;
        return RedirectToAction("index", new { message = message });
    }
}
```

Custom Model Binding Examples, using Attributes
----

```csharp
public class BoundUser
{
    [BindRequired]
    public int ID { get; set; }

    [BindRequired]
    public string FirstName { get; set; }

    [BindNever]
    public string LastName { get; set; }

    [BindRequired]
    public DateTime DateOfBirth { get; set; }

    [BindRequired]
    [FromRoute]
    public string Controller { get; set; }
}
public class BindingsController : Controller
{

    [HttpGet]
    public IActionResult Index([FromQuery] string message)
    {
        ViewData["message"] = message;
        return View();
    }

    [HttpGet]
    public IActionResult New([FromRoute] string controller)
    {
        BoundUser user = new BoundUser();
        user.Controller = controller;
        return View(user);
    }

    [HttpPost]
    public IActionResult New([FromForm] BoundUser user)
    {
        var message = user.FirstName + " " + user.LastName + ", Date of Birth: " + user.DateOfBirth.ToString("yyyy-MM-dd") + ", ID: " + user.ID + ", Controller: " + user.Controller;
        return RedirectToAction("index", new { message = message });
    }
}

```

Summary
----

- ASP.Net Core Data Binding has two methods: Default Binding and Custom Binding.

- Both can handle simple and complex types

- Default Binding uses Reflection and Recursion for complex types.

- Custom Binding uses specified which properties get bound, and from what data sources, via Attributes.






