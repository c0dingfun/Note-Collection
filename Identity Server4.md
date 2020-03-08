# Identity Server 4 for ASP.Net Core API

## [Identity Server 4 Concepts](https://identityserver4.readthedocs.io/_/downloads/en/latest/pdf/)

## Here are the Terminologies, related OpenID Connect and OAuth2, need to be understand.

- **User** - the human being the own of resources to be protected.
- **Client** - the software requests from IdentityServer4 the user's identity (via identity token) or/and for accessing a protected resource (via access token)
- Note: client also has identity (Client Identity) and must be register with IdentityServer4 before it can request tokens.
- Example of clients are web application, native mobile or desktop application, SPA, and server process, etc.
- **Resources** are something you want to protect with IdentityServer4 such as **identity data** and APIs.
- **Identity data** is identity information (ask claims) about a user: name or email address.
- APIs is API resources represent functionality a client want invoke--like the WebAPI.
- **Identity Token** represents the outcome of an **Authentication** process. It contains at a bare minimum an identifier for a user (called **sub** aka subject claim) and information about how and when the user authenticated. It can contain additional identity data.
- **Access Token** allows access to an API resource. Clients request access tokens and forward them to the API. Access tokens contain information about the client and the user (if present). APIs use that information to authorize access to their data.

## Simple Flow: Protecting an API using Client Credentials

In this case, we setup an Identiy Server 4 (on port 5000) using ASP.Net Core and a WebAPI (the to-be-protected Resource) (on port 5001) also using ASP.Net Core.

### Setup IdentityServer4

- Create a project using  ASP.NET Core Web Application - Empty template.

- Update the launchSetting.json to have the IentityServer4 using port 5000

```json
    "IdentityServer": {
      "commandName": "Project",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:5000"
    }
```

- Use Code-as-Configuration to define Resource (API) and Client:

    * provides to static methods to:
    * Get API resources to be protected.
    * Get Clients - using ClientCredentials (id + secret)

```csharp
    /// <summary>
    /// Code as Configuration
    /// </summary>
    public class Config
    {
        public static IEnumerable<ApiResource> GetAllApiResource()
        {
            return new List<ApiResource>
            {
                new ApiResource(name:"WebAPI", displayName: "Customer API for Web API")
            };
        }

        public static IEnumerable<Client> GetClients()
        {
            return new List<Client>
            {
                new Client
                {
                    ClientId = "client", // unique Id of the client (like login)
                    ClientSecrets = { new Secret("secret".Sha256()) }, // like password
                    AllowedGrantTypes = GrantTypes.ClientCredentials,
                    AllowedScopes = {"WebAPI"} // add to the collection to indicate which WebAPI is allowed
                }
            };
        }
    }
```

- Configure the Identity Server in Startup.cs:

```csharp
    public class Startup
    {
        // This method gets called by the runtime. Use this method to add services to the container.
        // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddIdentityServer()
                    .AddDeveloperSigningCredential()    // <<<
                    .AddInMemoryApiResources(Config.GetAllApiResource()) // <<<
                    .AddInMemoryClients(Config.GetClients()); // <<<
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseRouting();

            app.UseIdentityServer(); //<<<<
        }
    }
```

- Run the IdentityServer4

- Use Postman or a Browser at
- http://localhost:5000/.well-known/openid-configuration
- to retrieve the **discovery document** which will be used by clients and APIs to download the necessary configuration data.

```iecst
{
    "issuer": "http://localhost:5000",
    "jwks_uri": "http://localhost:5000/.well-known/openid-configuration/jwks",
    "authorization_endpoint": "http://localhost:5000/connect/authorize",
    "token_endpoint": "http://localhost:5000/connect/token",
    "userinfo_endpoint": "http://localhost:5000/connect/userinfo",
    "end_session_endpoint": "http://localhost:5000/connect/endsession",
    "check_session_iframe": "http://localhost:5000/connect/checksession",
    "revocation_endpoint": "http://localhost:5000/connect/revocation",
    "introspection_endpoint": "http://localhost:5000/connect/introspect",
    "device_authorization_endpoint": "http://localhost:5000/connect/deviceauthorization",
    "frontchannel_logout_supported": true,
    "frontchannel_logout_session_supported": true,
    "backchannel_logout_supported": true,
    "backchannel_logout_session_supported": true,
    "scopes_supported": [
    "WebAPI",
    "offline_access"
    ],
    "claims_supported": [],
    "grant_types_supported": [
    "authorization_code",
    "client_credentials",
    "refresh_token",
    "implicit",
    "urn:ietf:params:oauth:grant-type:device_code"
    ],
    "response_types_supported": [
    "code",
    "token",
    "id_token",
    "id_token token",
    "code id_token",
    "code token",
    "code id_token token"
    ],
    "response_modes_supported": [
    "form_post",
    "query",
    "fragment"
    ],
    "token_endpoint_auth_methods_supported": [
    "client_secret_basic",
    "client_secret_post"
    ],
    "id_token_signing_alg_values_supported": [
    "RS256"
    ],
    "subject_types_supported": [
    "public"
    ],
    "code_challenge_methods_supported": [
    "plain",
    "S256"
    ],
    "request_parameter_supported": true
}
```

- Then, for testing the working of the Identity Server 4, use Postman (not browser) to 

- [Get Access Token](#GAT) POST http://localhost:5000/connect/token to retrieve the **access token**

- with Body:

    * x-www-form-urlencoded

    |key|value|
    |--|--|
    |grant_type|client_credentials|
    |scope|WebAPI|

- **Access Token** looks:

```json
{
    "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImRjdVBMYng5OGtyR2tzNElEclI1NHciLCJ0eXAiOiJhdCtqd3QifQ.eyJuYmYiOjE1ODM2MjkzOTMsImV4cCI6MTU4MzYzMjk5MywiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo1MDAwIiwiYXVkIjoiV2ViQVBJIiwiY2xpZW50X2lkIjoiY2xpZW50Iiwic2NvcGUiOlsiV2ViQVBJIl19.T9bZ_FceEoH1BBGmslp9i_GiquLVYG6C5c6Rg1copIBEPjn7ftQYhvQOTi5e5ixu9wh2NMm0hJJwXry_K1WwI9HTSzc6dsbB_EOFqkLRDZ5UUPMWS2NnZF2847a0PyGJDL4J7uLcxF8aDAQyYEcE8t0zXN9K2S05dQ6odv0162_jjTVyWRC1YV7tNq4-_nS39VFmVuXu804RciLvufzhycPRwMs1mWH8XJ5uVw-HLr-hLq1FL-OxSfUbTTCdmz6pK39pCHDC8xMj74CoG2QvEEegknpWiw_801hMAil8XAHyI3_uI1KST7k2yVVBuZ_iiddQCQRSiOVeT9BNJWk1xA",
    "expires_in": 3600,
    "token_type": "Bearer",
    "scope": "WebAPI"
}
```

### Setup WebAPI

- Create the WebAPI using ASP.Net Core Web Application - API template

- Create the API Controller and all its needed models and DbContext [Abbreviated]

```csharp
    [Authorize] // to require authentication to access the API
    [Route("api/[controller]")]
    [ApiController]
    public class CustomersController : ControllerBase   // API Controller use ControllerBase, not Controller, which is used for Controller+View

    // ...
```

- NuGet IdentityServer4.AccessTokenValidation package

- NuGet EntityFrameworkCore.InMemory package, so that we can create InMemory DB in this WebAPI implementation.

- Setup launchSettings.json to have the WebAPI to use port 5001

```json
    "WebAPI": {
      "commandName": "Project",
      "launchBrowser": true,
      "launchUrl": "api/Customers",
      "applicationUrl": "http://localhost:5001",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
```

- Setup Identity Authority in Startup.cs

```csharp
   public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddAuthentication(defaultScheme:"Bearer")  // <<< return AuthenticationBuilder
                    .AddIdentityServerAuthentication(options =>
                    {
                        options.Authority = "http://localhost:5000";    // <<< specify Identity Server address
                        options.RequireHttpsMetadata = false;
                        options.ApiName = "WebAPI";     // <<< The resource name

                    });


            // use EF's InMemoryDB
            services.AddDbContext<CustomerContext>(opts =>
            {
                opts.UseInMemoryDatabase(databaseName: "CustomerDB");

            }); 

            services.AddControllers();
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseRouting();

            app.UseAuthentication(); // <<< add the authentication middleware to the pipeline
            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });
        }
    }
}
```

- Run the WebAPI without [Authorize] attribute on the Controller first
- Test the API endpoints are working

- Then, put back the [Authorize] attribute on the Controller
- Test the API endpoints; they should all get "401 Unauthorized" return code -- using PostMan

- In order to be able to access the WebAPI, we need to use Identity Server to get the **access token" first:

1. Get the **Access Token** from Identity Server (see above on how to get it)

2. Setup HTTP Headers

    |key|value|
    |--|--|
    |Content-Type|application/json|
    |Authorization|Bearer <Paste the **Access Toake** here>|

3. Make the API request: like http://localhost:5001/api/customers

- It should return 200 OK return code.

### Create the Console Client

Instead of using the Postman as the generic testing tool, we can create a Console client, that will obtained the IdentityServer's endpoint to get the "access token", then use the "access token" to access the WebAPI.

- The Console Client

```csharp
 class Program
    {
        //static void Main(string[] args)
        //{
        //    Console.WriteLine("Hello World!");
        //}

        const string IdendityServer4Address = "http://localhost:5000";
        const string WebApiEndPoint= "http://localhost:5001/api/customers";

        // clever way to do async Main()
        static void Main(string[] args) => MainAsync().GetAwaiter().GetResult();

        private static async Task MainAsync()
        {
            // discover Identity Server 4's endpoints via its discovery document
            var client = new HttpClient(); // Note: Both DiscoveryClient and TokenClient were deprecated
            DiscoveryDocumentResponse disco = await client.GetDiscoveryDocumentAsync(IdendityServer4Address);

            if (disco.IsError)
            {
                Console.WriteLine(disco.Error);
                return; // without "discovery document" we can not go on
            }

            // obtain the access token, for this client
            TokenResponse tokenResponse = await client.RequestClientCredentialsTokenAsync(
                    new ClientCredentialsTokenRequest
                    {
                        Address = disco.TokenEndpoint,
                        ClientId = "client",
                        ClientSecret = "secret",
                        Scope = "WebAPI"
                    }
                );

            if (tokenResponse.IsError)
            {
                Console.WriteLine(tokenResponse.Error);
                return; // without "access token" we can not go on
            }

            Console.WriteLine(tokenResponse.Json);
            Console.WriteLine("\n\n");

            // Ready to consume the WebAPI 
            var apiClient = new HttpClient();
            apiClient.SetBearerToken(tokenResponse.AccessToken);

            var customerInfo = new StringContent(   // construct the payload for adding new customer
                JsonConvert.SerializeObject(
                        new
                        {
                            Id = 12,
                            FirstName = "Kenny",
                            LastName = "Doe"
                        }),
                        Encoding.UTF8,
                        "application/json");

            // post to the WebAPI to add new customer
            HttpResponseMessage createCustomerResponse = await apiClient.PostAsync(WebApiEndPoint, customerInfo);

            if (!createCustomerResponse.IsSuccessStatusCode)
            {
                Console.WriteLine(createCustomerResponse.StatusCode);
            }
            else
            {
                Console.WriteLine("Added new Custeromer");
            }

            // from the WebAI, get a list of the customers 
            HttpResponseMessage getCustomerReponse = await apiClient.GetAsync(WebApiEndPoint);
            if (!getCustomerReponse.IsSuccessStatusCode)
            {
                Console.WriteLine(getCustomerReponse.StatusCode);
            }
            else
            {
                string content = await getCustomerReponse.Content.ReadAsStringAsync();
                Console.WriteLine(JArray.Parse(content));
            }

            Console.ReadLine();
        }
    }
```

- The Result

```iecst
{
  "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImRjdVBMYng5OGtyR2tzNElEclI1NHciLCJ0eXAiOiJhdCtqd3QifQ.eyJuYmYiOjE1ODM2MzgzNzUsImV4cCI6MTU4MzY0MTk3NSwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo1MDAwIiwiYXVkIjoiV2ViQVBJIiwiY2xpZW50X2lkIjoiY2xpZW50Iiwic2NvcGUiOlsiV2ViQVBJIl19.O58TtZGx12Cb-YO_nLiogYFafQvQbuSnMCE3rj92k8L-XcfX74DfPiW6Rh_zx_HjX3l1qk526F5qShJt2t3knAgqKQJEq2oapzBeueGiur83G3YkuOc3lnrp5mJWR-NhZraZqqIOfUrrhhPCZRefb_oPBjSsmSwWD7LxafISLmD5i9EDehSERV-8bZCh0UZMToJX0E15HbAaMZmCu6OupRgrmI9VJW_KhJ--VQFWVoHE57IxefmwU97xGIW7ENBti_aZW9dPWsEQzxsISO9mYI1wg4wSs8eWglW614bF7SSTiX5tm6opotLKYQ_hX7fWlELnUoP_tjZQE53xpfsxIQ",
  "expires_in": 3600,
  "token_type": "Bearer",
  "scope": "WebAPI"
}

Added new Custeromer

[
  {
    "id": 1,
    "firstName": "John",
    "lastName": "Doe"
  },
  {
    "id": 2,
    "firstName": "Kevin",
    "lastName": "Smith"
  },
  {
    "id": 10,
    "firstName": "Richard",
    "lastName": "Date"
  },
  {
    "id": 4,
    "firstName": "xxx",
    "lastName": "yyy"
  },
  {
    "id": 5,
    "firstName": "abc",
    "lastName": "efg"
  },
  {
    "id": 11,
    "firstName": "hij",
    "lastName": "klm"
  },
  {
    "id": 12,
    "firstName": "test",
    "lastName": "webapi"
  }
]

```
