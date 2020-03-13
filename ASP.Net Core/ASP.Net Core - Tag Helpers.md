# ASP.Net Core - Tag Helpers

What is a Tag Helper?
----

- It's a set of code replacing the OLD Razor helpers to build your cshtml forms.

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

```csharp
    <a asp-action="Index" asp-controller="Movie">Back to Movies</a>
    // a few more tag helpers can be applied to <a> anchor tag:
    // asp-fragment, asp-route, asp-path ...
```

Form Tag Helper
----

```csharp
    <form asp-action="Add" asp-anti-forgery="true" asp-controller="Movie"></form>
```

Label Tag Helper
----

```csharp
    <label asp-for="Title"></label> // asp-for="<Specified Model property>"
```

Input Tag Helper
----

```csharp
    <input type="text" asp-for="Title" asp-format="{0:N4}"/> // 1.1200
```

TextArea Tag Helper
----

```csharp
     <textarea asp-for="Description"></textarea>
```

Select Tag Helper
----

```csharp
    <select asp-for="SelectedGenre" asp-items="Model.AllGenres"></select>
```

- why "asp-for" does not require **Model** while asp-items does?

Validation Tag Helper
----

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
    asp-fallback-href="~/lib/bootstrap/css/bootstrap.min.css" asp-fallback-test-class="hidden" asp-fallback-test-property="visibility" asp-fallback-test-value="hidden" />

    <script src="//ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js" asp-fallback-src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.js" asp-fallback-test="window.jQuery"></script>

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

- Instructs the server (server-side) to cache the content in memory, set with an expiration date.

```csharp
    <cache expires-after="@TimeSpan.FromMinutes(5)" vary-by-user="true">
        @Html.Partial("WeatherReport")
    </cache>
```


