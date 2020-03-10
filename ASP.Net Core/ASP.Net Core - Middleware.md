# ASP.Net: Creating Custom Middleware

Key Points
----

- ASP.Net Core builds its request/response pipeline using Middleware.

- A Middleware is nothing more than a POCO class which together with other Middleware form a "pipeline" which handles requests and responses.

- The pipeline ends at the first call to app.Run().

- To add pieces of Middleware to the pipeline, use app.Use(). [This way, we don't need to create a POCO class for the Middleware]; or

- Use either of the following Extension Methods

- app.UseMiddleware<TMiddleware>(); or

- app.UseMiddleware(typeof(TMiddleware));

- Or better yet, create your own Extension Methods which encapsulate the app.UseMiddleware(...). And we can use the Extension Method in the Startup.cs' Configure() method to insert the Custom Middleware into the pipeline.
