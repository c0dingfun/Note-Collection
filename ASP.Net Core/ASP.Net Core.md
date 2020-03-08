# ASP.Net Core

- Can control your application pipeline using Middlewares, which you configure and set it up, via Dependency Injection and IApplicationBuilder.Use()/Map()/MapWhen()/Run().

## Benefits

- A unified story for building web UI and web APIs.
- Architected for testability.
- Razor Pages makes coding page-focused scenarios easier and more productive.
- Blazor lets you use C# in the browser alongside JavaScript. Share server-side and client-side app logic all written with .NET.
- Ability to develop and run on Windows, macOS, and Linux.
- Open-source and community-focused.
- Integration of modern, client-side frameworks and development workflows.
- Support for hosting Remote Procedure Call (RPC) services using gRPC.
- A cloud-ready, environment-based configuration system.
- Built-in dependency injection.
- A lightweight, high-performance, and modular HTTP request pipeline.
- Ability to host on the following:

1. Kestrel
2. IIS
3. HTTP.sys
4. Nginx
5. Apache
6. Docker

- Side-by-side versioning.
- Tooling that simplifies modern web development.

## Build web APIs and web UI using ASP.NET Core MVC

- The Model-View-Controller (MVC) pattern helps make your web APIs and web apps testable.
- Razor Pages is a page-based programming model that makes building web UI easier and more productive.
- Razor markup provides a productive syntax for Razor Pages and MVC views.
- Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.
- Built-in support for multiple data formats and content negotiation lets your web APIs reach a broad range of clients, including browsers and mobile devices.
- Model binding automatically maps data from HTTP requests to action method parameters.
- Model validation automatically performs client-side and server-side validation.

## ASP.Net - Packt - Getting Started with ASP.Net Core MVC

### Major Interfaces

IWebHost:
    Server Features (IFeatureCollection)
    Services (IServiceProvider)
    Start()
    StartAsync(CancellationToken)
    StopAsync(CancellationToken)
    ...

IWebHostBuilder:
    Build() -  build an IWebHost which hosts a web application
    ConfigureAppConfiguration(Action<WebHostBuilderContext, IConfigurationBuilder>)
    ConfigureServices(Action<IServiceCollection>)
    ConfigureServices(Action<WebHostBuilderContext, IServiceCollection>)
    GetSetting(string)
    UseSetting(String, String)
    ...

Program.c: (Main())

```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>     // IWebHostBuilder
                {
                    webBuilder.UseStartup<Startup>();
                });
    }
```

IApplicationBuilder: (app)
    ApplicationServices
    Properties
    ServerFeatures

    Build()
    New()
    Use(Func<RequestDelete, RequestDelegate>)
    Run(httpContext) - terminate the pipeline

HttpContext:
    Request.
    Response.

RequestDelegate:

    public delegate System.Threading.Tasks.Task RequestDelegate(HttpContext context);


Startup.cs

```csharp
   public class Startup
    {
        // This method gets called by the runtime. Use this method to add services to the container.
        // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
        public void ConfigureServices(IServiceCollection services)
        {
            // HH: DI the services
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseRouting();

            // HH:
            Hello hello = new Hello();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapGet("/", async context =>
                {
                    //await context.Response.WriteAsync("Hello World from Kenny!");
                    await context.Response.WriteAsync(hello.SayHello());
                });
            });
        }
```

[ASP.Net Core](https://www.tutorialsteacher.com/core)
