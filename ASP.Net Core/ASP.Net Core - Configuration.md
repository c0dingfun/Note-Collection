Configuration in ASP.Net Core
====

- Configuration is set up as part of the **WebHost.CreateDefaultBuilder** method called in Program.cs. 

- Various key/value stores are added to configuration by default:

    1. appsettings.json (and another version named after the current environment e.g. appsettings.Development.json)

    2. User Secrets (if the environment is Development)

    3. Environment variables

    4. Command line arguments

- Other stores such as XML files, .ini files and so on, can be used, if required.
- Configuration is added to the Dependency Injection system and is accessible throughout the application via an **IConfiguration** object.

AppSetting.Json
----

- Each configuration setting is stored in its own section.

```json
{
  "ConnectionStrings": {    // connection string section
    "DefaultConnection": "Data Source=app.db"
  },
  "Logging": {              // logging section
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  }
}
```

Custom Settings
----

- AppSetting.json can be extended with Custom Settings, by providing any unique section name.

```json
{
    // ...
    "AppSettings":{     // Custom Settings
    "First" : "Value 1",
    "Second" : "Value 2",
    "Car": {
        "NumberOfDoors" : 5,
        "RegistrationDate" : "2017-01-01T00:00:00.000Z",
        "Color" : "Black"
        }
    }
}
```

How to Access Configuration Settings (Programmatically)
----

- Three (3) Approaches:

String-based Approach (1)
----

- Reference a value using a string-based approach. 

- The **IConfiguration** object enables you to access configuration settings in a variety of ways once it has been injected into your PageModel's constructor. 

- Need to add a using directive for **Microsoft.Extensions.Configuration** to the PageModel class file. 

- The section is specified and subsequent properties are referenced by separating them with colons (:)

```csharp
    private readonly IConfiguration _configuration;
    public IndexModel(IConfiguration configuration) // injected
    {
        _configuration = configuration;
    }
    public void OnGet()
    {
        ViewData["config"] = _configuration["AppSettings:First"];   // string-base approach
    }
```

- String-Baed approach is very error-prone; it is a typo mistake away from a **NullReferenceException** during runtime.

Connection Strings
---

- Connection String is a special case, in **Configuration** class, there is a convenient method for retrieving connection strings: **GetConnectionStrings**, which takes the name of the connection.

```csharp
    var connString = Configuration.GetConnectionString("DefaultConnection");
```

Strongly Typed Approach (2)
----

- A more robust approach is using the Configuration system's built-in capability to bind settings to a C# object.

- With this:

```json
    {
        // ...
        "AppSettings":{     // Custom Settings
        "First" : "Value 1",
        "Second" : "Value 2",
        "Car": {
            "NumberOfDoors" : 5,
            "RegistrationDate" : "2017-01-01T00:00:00.000Z",
            "Color" : "Black"
            }
        }
    }

```

- First define the C# classes represent the Configuration in the Json file

```csharp
    public class AppSettings
    {
        public string First { get; set; }
        public string Second { get; set; }
        public Car Car { get; set; }
    }
    public class Car
    {
        public int NumberOfDoors { get; set; }
        public DateTime RegistrationDate { get; set; }
        public string Color { get; set; }
    }
```

- Now, use **IConfiguration.GetSection** method to bind the content of appsettings.json to get an instance of C# Setting class (ie: **AppSettings**)

```csharp
    private readonly IConfiguration _configuration;
    public IndexModel(IConfiguration configuration)
    {
        _configuration = configuration;
    }
    public void OnGet()
    {
        var settings = _configuration
                            .GetSection("AppSettings")  // get the Setting section in Json
                            .Get<AppSettings>();        // get the Instance of a section class

        // Accessing the setting in Strongly typed fashion
        ViewData["RegistrationDate"] = settings
                                        .Car
                                        .RegistrationDate;  
    }
```

The Options Pattern (3)
----

- Similar to Strongly Typed Approach
- Is used to group related configuration values together in classes

```json
    {
        "Logging": {                // logging
            "LogLevel": {
            "Default": "Warning"
            }
        },
        "AllowedHosts": "*",
        "Title": "My Great Site",   // title
        "Author": {                 // author
            "FirstName": "Mike",
            "LastName": "Brind"
        },
        "EmailFrom": "comments@mygreatsite.com",
        "EmailDisplayName": "Site Comments",
        "EmailSmtp" :  "localhost"
    }
```

- Grouping **Title** and **Author** using a class **MetaOptions**

```csharp
    public class MetaOptions
    {
        public string Title { get; set; }
        public Author Author { get; set; }
    }
    public class Author
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
```

- Grouping Email related in a class **EmailOptions**

```csharp
    public class EmailOptions
    {
        public string EmailFrom { get; set; }
        public string EmailDisplayName { get; set; }
        public string EmailSmtp { get; set; }
    }
```

- Make the above as service (can be injected later)

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<MetaOptions>(Configuration);
        services.Configure<EmailOptions>(Configuration);
        ...
    }
```

- The Use in PageModel: using **IOptions<TOptions>

```csharp
    ...
    using Microsoft.Extensions.Options;
    ...
    public class IndexModel : PageModel
    {
        private readonly MetaOptions_options;
        public IndexModel(IOptions<MetaOptions> options)
        {
            _options = options.Value;   // Note: Value to get the actual Option!!!
        }
        public string Title { get; set; }
        public Author Author { get; set; }
        public void OnGet()
        {
            Title = _options.Title;
            Author = _options.Author;
        }
    }
```

- The Use in Layout.cshtml: using @inject

```cshtml

    @inject Microsoft.Extensions.Options.IOptions<MetaOptions> metaOptions
    @{
        var options = metaOptions.Value;    // Note: Value to get the actual Option!!!
    }
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta name="author" content="@options.Author.FirstName @options.Author.LastName"/>
        <title>@options.Title</title>
        ...
```
