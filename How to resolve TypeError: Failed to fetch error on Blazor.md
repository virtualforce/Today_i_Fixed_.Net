# Problem
How to resolve System.Runtime.InteropServices.JavaScript.JSException 'TypeError: Failed to fetch' error on Blazor Webassembly?

# Environment
Visual Studio, Asp.Net Core 6, C#, Blazor Webassembly

# How you fix it
If your ASP.NET Core Web API project is hosting on separate site, please make sure you configured and enabled CORS to allow request(s) from your Blazor WebAssembly app, and make sure that API is running

# Solution
By Enabling CORS in ASP.NET Core Middleware

## To do that, let’s open the Startup.cs file in the server app and modify it:
```
private readonly string _policyName = "CorsPolicy";
```

## Add the below code in ConfigureServices(IServiceCollection services).

```
services.AddCors(opt =>
           {
                opt.AddPolicy(name: _policyName, builder =>
                {
                    builder.AllowAnyOrigin()
                        .AllowAnyHeader()
                        .AllowAnyMethod();
                });
            });
```
## Now add this code in Configure method. It is very important that we call this method after the UseRouting method and before the UseAuthorization method.

```
app.UseCors(_policyName);
```



## Run Application
Now run the application. With this policy, we enable access from any origin AllowAnyOrigin, allow any header in a request AllowAnyHeader, and allow any method AllowAnyMethod. 
