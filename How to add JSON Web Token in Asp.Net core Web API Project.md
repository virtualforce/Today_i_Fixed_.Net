# How to add JSON Web Token in Asp.Net core Web API Project

# Problem
Authentication has become mandetory of each and every application now a days. To have a fast a secure authentication, we can utilize JWT authentication.

# Environment
Visual Studio, Asp.Net Core, C#

# How you fix it
By configuring and utilizing JWT provided packages, after installing them. 

# Solution
First we have to install the following packages:
* Microsoft.AspNetCore.Authentication.JwtBearer 
* System.IdentityModel.Tokens.Jwt

Then we will have to add the following code to our startup class for the purpose of configuration.
```
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = "your-issuer",
            ValidAudience = "your-audience",
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("your-secret-key"))
        };
    });

```

Add the authentication middleware to the pipeline, by adding the following code
```
app.UseAuthentication();
```

Now let's move to the part of generation of JWT. We will have to use the System.IdentityModel.Tokens.Jwt package for creating JWT using the following code
```
var claims = new[]
{
    new Claim(ClaimTypes.Name, "username"),
    new Claim(ClaimTypes.Role, "role")
};
var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("your-secret-key"));
var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
var token = new JwtSecurityToken(
    issuer: "your-issuer",
    audience: "your-audience",
    claims: claims,
    expires: DateTime.UtcNow.AddMinutes(30),
    signingCredentials: credentials);
var tokenString = new JwtSecurityTokenHandler().WriteToken(token);

```

Now we can secure our API's by using the [Authorize] attribute, so that any incoming request not having bearer token in it's header will get declined.
