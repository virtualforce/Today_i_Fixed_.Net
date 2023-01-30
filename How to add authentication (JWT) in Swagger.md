# Problem
How to add authentication i.e JWT in Swagger?

# Environment
Visual Studio, Asp.Net Core 6, C#

# How you fix it
Installing the Swashbuckle, Microsoft.AspNetCore.Authentication.JwtBearer package and add the following custom code in the program.cs.

# Solution
## Installing packages
Install the required packages from the Nuget Package Manager or by simply running these commands in Package Manager Console.

```
dotnet add package Swashbuckle.AspNetCore
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

## Add the following code in Program.cs file
This code will help to enable authetication layer in the Swagger.

```
builder.Services.AddSwaggerGen(options => {
options.AddSecurityDefinition("oauth2", new OpenApiSecurityScheme{ 
        Description = "Authentication using Bearer Scheme (\"bearer {token}\")",
        In = ParameterLocation.Header,
        Name = "Authorization",
        Type = SecuritySchemeType.ApiKey
});
    options.OperationFilter<SecurityRequirementsOperationFilter>();
});
```
## Now add the following code in Program.cs file
This code will take API key from the AppSettings, help to generate JWT.

```
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8
            .GetBytes(builder.Configuration.GetSection("AppSettings:Token").Value)),
            ValidateIssuer = false,
            ValidateAudience = false   

        };
    });
```
## Fianlly add these lines in the following order in the end of Program.cs file

```
app.UseAuthentication();

app.UseAuthorization();
```

## Run Application
Now run the application, you will be able to see lock sign in the top of swagger API.
put the jwt token in this format 'Bearer "your JWT Token"'.
