# ASP.Net Core - Tag Helpers

[Ref Doc](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-3.1#the-form-tag-helper)

What is a Tag Helper?
----

- It's a set of tag helpers (eg: asp-action) replacing the OLD Razor HTML helpers (eg: @HTML) to build your cshtml forms.

- Tag Helpers provide us with means to change and enhance existing HTML elements in our views, which processed by the Razor Templating engine which in return creates an HTML and serves it to the browser.

- Example: Old, using Razor helper

```csharp
    @Html.ActionLink("Add a Movie", "Add", "Movie")
```

- Example: Now, using tag helper

```csharp
    <a asp-action="Add" asp-controller="Movie">Add a Movie</a>
```

- Using tag helper, we can now write Views that look more like HTML rather than an unwieldy mix of HTML and C#.

Anchor Tag Helper
----

- An alternative to using @Html.ActionLink or @Url.Action

```csharp
    @Html.ActionLink("Register", "Register", "Account")
    <a href="@Url.Action("Register", "Account")">Register</a>

    // generate identical html
    <a href="/Account/Register">Register</a>
    <a href="/Account/Register">Register</a>
```

- Using **asp-controller** and **asp-action** attributes

```csharp
    <a asp-action="Register" asp-controller="Account">
    // a few more tag helpers can be applied to <a> anchor tag:
    // asp-fragment, asp-route, ...
```

- Adding Route parameters

    + You might need to specify additional parameters for the controller action that you are binding to. Use **asp-route-** prefix:

    ```html
        <a
            asp-controller="Product"
            asp-action="Display"
            asp-route-id="@ViewBag.ProductId">
            View Details
        </a>

        // generate
        <a href="/Product/Display/1">View Details</a>
 
    ```

- Use **Named** Routes

    ```csharp
        routes.MapRoute(
            name: "login",
            template: "login",
            defaults: new { controller = "Account", action = "Login" });

        <a asp-route="login">Login</a>
    ```

- Protocol, Host and Fragment

    + The anchor tag helper also has options for specifying the protocol, host and fragment for a generated URL.
    + These are useful if you want to override the defaults to fore https:// or link to a particular portion of a page.
    + Old Way is using @Html.ActionLink(....)
    + With tag helper, you can use any combination of asp-protocol, asp-host and asp-fragment attributes. The result is a much cleaner syntax that is easier for the reader to understand.

    ```html
        <a
            asp-controller="Account"
            asp-action="Register"

            asp-protocol="https"
            asp-host="asepecificdomain.com"
            asp-fragment="fragment">Register</a>

        <!--or with the protocol only -->
        <a
            asp-controller="Account"
            asp-action="Register"
            asp-protocol="https">Register</a>
   
    ```
   




Form Tag Helper
----

- Better than @using(Html.BeginForm()){....}

```html
    <form method="post"
        asp-action="Add"
        asp-controller="Movie"
        asp-anti-forgery="true">
        ...
    </form>
```

- Adding Parameters for Controller Action

    + You might need to specify additional parameters for the controller action that you are binding to. You can specify values for these parameters by adding attributes with the **asp-route-** prefix.

    ```html
        <form
            asp-controller="Account"
            asp-action="Login"
            asp-route-returnurl="@ViewBag.ReturnUrl"
            method="post" >
        </form>
    ```

- Binding to a Named route

```csharp
    routes.MapRoute(
        name: "login",
        template: "login",
        defaults: new { controller = "Account", action = "Login" });
```

```html
    <form
        asp-route="login"
        asp-route-returnurl="@ViewBag.ReturnUrl"
        method="post" >
    </form>

```



Label Tag Helper
----

```csharp
    <label asp-for="Title"></label> // asp-for="<Specified Model property>"
```

Input Tag Helper
----

- Model Property type

```csharp

    @Html.EditorFor(m=>m.TheProperty); // replaced with tag helper

    <input type="text" asp-for="Title" asp-format="{0:N4}"/> // 1.1200

    public class SimpleMOdel
    {
        public string Email {get; set;}
    }

    <input asp-for="Email"/>

    // Razor templating engine generates:
    <input type="text" id="Email" name="Email" value="theEmail@fromTheModel.com>

```

- the "id" and "name" are added based on the property name specified in the asp-for attribute. The "type" of the input was set to "text" because the Email property is a string. If the property specified is a bool, the the tag helper will generate a input of type "checkbox"

|.Net Type | Input Type|
|--|--|
|String|type="text"|
|DateTime|type="datetime"|
|Byte|type="number"|
|int16, int32, int64|type="number"|
|single, double|type="number"|
|boolean|type="checkbox"

- Validation and Model Property Attribute

    + In addition to the asp-for aware of property types, it is also aware fo common dta annotations for special types and validation.

```csharp
    public class SimpleModel
    {
        [Required]
        public string Email {get; set;}
    }

    <input asp-for="Email"/>

    // Razor templating engine generates:
    <input
        type="text"
        id="Email"
        name="Email"
        value="theEmail@fromTheModel.com
        data-val="true"
        data-val-required="The Email field is required."
        />

    // data-val-* are jQuery Validation

    // Adding [EmailAddress] Attribute to SimpleModel.Email
    <input
        type="Email"
        id="Email"
        name="Email"
        value="theEmail@fromTheModel.com
        data-val="true"
        data-val-required="The Email field is required."
        />

    // And jQuery will validate if the email in the right format
```

|Attribute|Input Type|
|--|--|
|[EmailAddress]|type="email"|
|[Url]|type="url"|
|[HiddenInput]|type="hidden"|
|[Phone]|type="tel"|
|[DataType(DataType.Password)]|type="password"|
|[DataType(DataType.Date)]|type="date"|
|[DataType(DataType.Time)]|type="time"|

- Navigating Child Properties

    + you can also navigate to child properties of your view model.

```csharp
    public class AddressViewModel
    {
        public string AddressLine1{get;set;}
    }
    public class RegisterViewModel
    {
        public string UserName {get; set;}
        public AddressViewModel Address {get; set;}
    }

    <input asp-for="Address.AddressLine1">

    // will generate following HTML
    <input
        name="Address.AddressLine1"
        id="Address.AddressLine1"
        type="text"
        value="">
```

- Formatting Values

    + asp-format="{0:N4}" uses standard .Net string format

```html
    <input asp-for="SomeProperty" asp-format="{0:N4}"/>
```

TextArea Tag Helper
----

```csharp
     <textarea asp-for="Description"></textarea>
```

Select Tag Helper
----

- An alternative to the @Html.DropDownListFor()

```csharp
    using Microsoft.AspNetCore.Mvc.Rendering;
    using System.Collections.Generic;

    namespace FormsTagHelper.ViewModels
    {
        public class CountryViewModel
        {
            public string Country { get; set; }

            public List<SelectListItem> Countries { get; } = new List<SelectListItem>
            {
                new SelectListItem { Value = "MX", Text = "Mexico" },
                new SelectListItem { Value = "CA", Text = "Canada" },
                new SelectListItem { Value = "US", Text = "USA"  },
            };
        }
    }
    <select asp-for="Country" asp-items="Model.Countries"></select>
    // why Model is not used in asp-form?
    // Because:
    //The asp-for attribute value is a special case and doesn't require a Model prefix, the other Tag Helper attributes do (such as asp-items)

    // Multi-select is generated if the property in the asp-for attribute is an IEnumerable.
    public class SimpleViewModel
    {
        public IEnumerable<string> CountryCodes { get; set; }
    }

    <select asp-for="CountryCodes"
            asp-items="ViewBag.Countries">
    </select>

    // Generated HTML:
    <select name="CountryCodes"
            id="CountryCodes"
            multiple="multiple">
        <option selected="selected" value="CA">Canada</option>
        <option value="USA">United States</option>
        <option value="--">Other</option>
    </select>


```

Validation Tag Helper
----

- two Validation tag helpers:
    1. Validation Message tag helper
        + To display validation message for a specific property of your model class.
        + Simply add the "asp-validation-for" attribute to a span element.

        ```html
            <span asp-validation-for="Email"></span>

            // will generate a span containing validtaion message for the specified property. Assuming the "Email" property has a [Required] attribute

            <span
                class="field-validation-eror"
                data-valmsg-replace="true"
                data-valmsg-for="Email">
                TheEmail field is requried.
            </span>

            // Note: validation message tag helper is a alternative to
            // @Html.ValidationMessageFor(m=>m.Email) the html helper method
        ```

    2. Validation Summary tag helper

        + To display validation messages that applied to your entire mode.
        + Can optionally specify to include all property level validation messages in the summary or only display the messages that apply at the model level.
        + To use it, by add the asp-validation-summary attribute to a `<div>` element.
        + The value of the attribute can be: ValidationSummary.All or ValidationSummary.ModeOnly or ValidationSummary.None
        + All - will display both property and model level validation messages.
        + ModelOnly - will display only validation messages that applied to model level.
        + None - do nothing.
        + An alternative to @Html.ValidationSummary(true) html helper method

        ```html
            <div asp-validation-summary="all"></div>

            // generate following html
            <div
                class="validation-summary-valid"
                data-valmsg-summary="true">
                <ul>
                    <li style="display: none;"></li>
                </ul>
            </div>

            <div
                class="validation-summary-errors"
                data-valmsg-summary="true">
                <ul>
                    <li>The Email field is required.</li>
                    <li>The Password field is required.</li>
                </ul>
            </div>
        ```

```csharp
    <span asp-validation-for="Description"></span>
```

Environment Tag Helper
----

```csharp
    <environment names="Development">
    ...
    </environment>
    <environment names="Staging,Production">
    ...
    </environment>
```

Link and Script Tag Helpers
----

```csharp
    <link rel="stylesheet" href="//ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css"
    asp-fallback-href="~/lib/bootstrap/css/bootstrap.min.css"
    asp-fallback-test-class="hidden"
    asp-fallback-test-property="visibility"
    asp-fallback-test-value="hidden" />

    <script src="//ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js"
    asp-fallback-src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.js" asp-fallback-test="window.jQuery">
    </script>


    <script
        asp-src-include="/js/about/**/*.js"
        asp-src-exclude="/js/about/a/**">
    </script>
```

Image Tag Helper
----

```csharp
    <img src="~/content/images/profile.jpg"
        alt="John Smith"
        asp-append-version="true" />
```

- the asp-append-version append the version info of the image in generated HTML

```html
    <img src="/content/images/profile.jpg?v=X5q6D366_nQ2fQqUso0F24gWy2ZekXjHz83KmWyaiOOk"
        alt="John Smith"/>
```

- with the appended version if an image is changed, it helps to force the browser to re-download it. The technique is called **cache bursting**.

Cache Tag Helper
----

- Purely a server-side tag

- Wraps arbitrary content and allows those contents to be cached in memory based on the parameters specified.

- Instructs the server (server-side) to cache the content in memory, set with an expiration date.

- How it works

    + Simply wrap the contents you want cached with a `<cache>` tag and the contents of the tag will be cached in memory. Before processing the contents of the cache tag, the tag helper will check to see if the contents have been stored in the MemoryCache. If the contents are found in the cache, then the cached contents are sent to Razor. If the contents are not found, then Razor will process the contents and the tag helper will store it in the memory cache for next time. By default, the cache tag helper is able to generate a unique ID based on the context of the cache tag helper.

```html
    // expires-after
    <cache 
        expires-after="@TimeSpan.FromMinutes(5)" 
        vary-by-user="true">
        @Html.Partial("WeatherReport")
        *last updated @DateTime.Now.ToLongTimeString()
    </cache>

    // expires-on
    <cache 
        expires-on="@DateTime.Today.AddDays(1).AddTicks(-1)">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>

    // expires-sliding (not being access by specific time period)
    <cache expires-sliding="@TimeSpan.FromMinutes(5)">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>
```

- Vary-by / complex cache

    ```html
    <cache vary-by-user="true"> 
        <!--View Component or something that gets data from the database--> 
        *last updated @DateTime.Now.ToLongTimeString() 
    </cache>

    <cache vary-by-route="id">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>

    <cache vary-by-query="search">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>

    <cache vary-by-cookie="MyAppCookie">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>

    <cache vary-by-header="User-Agent">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>

    // fall back when other vary-by do not meet the needs
    // vary-by using specific value
    <cache vary-by="@ViewBag.ProductId">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>

    // complex keys
    <cache vary-by-user="true" vary-by-route="id">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>

    @using Microsoft.Framework.Caching.Memory

    <cache vary-by-user="true" 
        priority="@CachePreservationPriority.Low">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>


    // a cache tag are stored in an IMemoryCache which is limited by the amount of available memory. If the host process starts to run out of memory, the memory cache might purge items from the cache to release memory. In cases like this, you can tell the memory cache which items are considered a lower priority using the priority attribute.

    @using Microsoft.Framework.Caching.Memory
    <cache vary-by-user="true" 
        priority="@CachePreservationPriority.Low">
        <!--View Component or something that gets data from the database-->
        *last updated  @DateTime.Now.ToLongTimeString()
    </cache>

    ```

Custom Tag Helper
----

- [Ref Doc](https://www.davepaquette.com/archive/2015/06/22/creating-custom-mvc-6-tag-helpers.aspx "Details on Creating Custom Tag Helper")

- [Samples](https://github.com/dpaquette/TagHelperSamples)

- [Distributed Cache SQL Server or Redis](https://docs.microsoft.com/en-us/aspnet/core/performance/caching/distributed?view=aspnetcore-3.1)